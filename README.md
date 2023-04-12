# RFM-Analysis

What is CRM and RFM?

CRM is the management of a company's relationship with its customers.

RFM enables customers to be divided into groups based on their purchasing habits, and enables the development of strategies specific to each group.

RFM Metrics:
Recency: The time elapsed since the customer's last purchase.
Frequency: The number of purchases made by the customer.
Monetary: The total amount of money spent by the customer on purchases.
If we write these three metrics in the same language, we obtain the RFM score.

We will create segments based on the scores obtained, using Recency and Frequency scores.


Dataset Details (Retail Dataset)

InvoiceID : ID of the transaction. A transaction might hold multiple records for the same customer at the same date with multiple products (SKU). DocumentID might be useful for combining the transactions and detecting the items sold together.

Date : Date of transaction / sell. In the date time format.

ProductID : Item / Product code. The unique code for each item sold.

TotalSales : Sales price for the transaction. If you want to get unit_price , divide TotalSales column to Quantity column

Discount : Discount amount for the transaction.

CustomerID : Unique customer id for each customer. For the data set, customer can be a reseller or a branch of the company.

Quantity : Number of items sold in the transaction.
