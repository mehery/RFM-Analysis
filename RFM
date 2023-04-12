import datetime as dt
import pandas as pd

pd.set_option('display.max_columns', None)
pd.set_option('display.max_rows', None)
pd.set_option('display.width', 500)
pd.set_option('display.float_format', lambda x: '%.3f' % x)

df = pd.read_csv("/kaggle/input/retail-data-set/file_out2.csv")



# We are trying to understand the data.

def check_df(dataframe, head=7):
    print("################### Shape ####################")
    print(dataframe.shape)
    print("#################### Info #####################")
    print(dataframe.info())
    print("################### Nunique ###################")
    print(dataframe.nunique())
    print("##################### NA #####################")
    print(dataframe.isnull().sum())
    print("################## Quantiles #################")
    print(dataframe.describe([0, 0.05, 0.50, 0.95, 0.99, 1]).T)
    print("#################### Head ####################")
    print(dataframe.head(head))

check_df(df)



# Data Preparation

# We are finding the total price of the product.
df["TotalPrice"] = df["TotalSales"] + df["Discount"] 

# We are finding the unit price of the product.
df["Unit_Price"] = df["TotalPrice"] / df["Quantity"]

# Since the value of these variables cannot be 0,
# we are removing those with a value of 0 from the data.
df = df[(df['Quantity'] > 0)]
df = df[(df["TotalPrice"] > 0)]
df = df[(df['Discount'] > 0)]       

# We are deleting the variable that does not carry any information.
df.drop("Unnamed: 0", inplace=True, axis=1)

df.head()



# Calculating RFM Metrics

# We are converting the object type date to the datetime structure.
df["Date"] = pd.to_datetime(df["Date"])    

df["Date"].max() 

# We are entering the date on which we prepared the data to calculate the Recency value
today_date = pd.to_datetime("2023-04-01")


rfm = df.groupby('CustomerID').agg({'Date': lambda Date: (today_date - Date.max()).days,    # recency degeri
                                     'InvoiceID': lambda Invoice: Invoice.nunique(),        # frequency degeri
                                     'TotalPrice': lambda TotalPrice: TotalPrice.sum()})    # monetary degeri


# We are changing the names of the variables we created.
rfm.columns = ['recency', 'frequency', 'monetary']

rfm.head()



# Calculating RFM Scores

# Since the smallest value in Recency is the most valuable, we start scoring from 5
rfm["recency_score"] = pd.qcut(rfm['recency'], 5, labels=[5, 4, 3, 2, 1])   

rfm["frequency_score"] = pd.qcut(rfm['frequency'].rank(method="first"), 5, labels=[1, 2, 3, 4, 5])

rfm["monetary_score"] = pd.qcut(rfm['monetary'], 5, labels=[1, 2, 3, 4, 5])


# We are creating the RFM score.
rfm["RFM_SCORE"] = (rfm['recency_score'].astype(str) +
                    rfm['frequency_score'].astype(str)) 

rfm.head()



# Creating & Analysing RFM Segments

seg_map = {
    r'[1-2][1-2]': 'hibernating',
    r'[1-2][3-4]': 'at_Risk',
    r'[1-2]5': 'cant_loose',
    r'3[1-2]': 'about_to_sleep',
    r'33': 'need_attention',
    r'[3-4][4-5]': 'loyal_customers',
    r'41': 'promising',
    r'51': 'new_customers',
    r'[4-5][2-3]': 'potential_loyalists',
    r'5[4-5]': 'champions'
}


# We are creating the correspondences of RFM scores in segments
rfm['segment'] = rfm['RFM_SCORE'].replace(seg_map, regex=True)     


# We are examining the RFM values corresponding to the segments
rfm.groupby("segment").agg({"recency":["mean","count"],
                            "frequency":["mean","count"],
                            "monetary":["mean","count"]})
                            
                            
rfm.head()                            



# We are creating a new dataframe.
new_df = pd.DataFrame()


# We can specify the desired segment in the new dataframe and access the IDs of the customers belonging to that segment
new_df["loyal_customers_id"] = rfm[rfm["segment"] == "loyal_customers"].index


new_df.head()
