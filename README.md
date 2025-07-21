# Chinook Customer Segmentation (SQL + Power BI)

A project that showcases my **SQL** and **Power BI** skills using the **Chinook database**, a sample dataset for digital music stores.

## Table of Contents

1. [Project Overview](#project-overview)  
2. [Project Objective](#project-objective)  
3. [Dataset Overview](#dataset-overview)  
4. [Tools Used](#tools-used)  
5. [Business Insights](#business-insights)  
6. [Power BI Dashboard](#power-bi-dashboard)  
7. [SQL Logic Summary](#sql-logic-summary)  
8. [SQL Queries Used](#sql-queries-used)  
9. [Skills Demonstrated](#skills-demonstrated)  
10. [Data Source](#data-source)  
11. [Acknowledgements](#acknowledgements)  
12. [Recommendations](#recommendations)  
13. [Conclusion](#conclusion)  
14. [Let's Connect](#lets-connect)


## Project Overview

This project explores customer segmentation using the Chinook music store database. It applies SQL and Power BI to uncover purchasing patterns, segment VIP customers, and analyze geographic revenue trends. The goal is to generate insights that support marketing, retention, and business strategy.


## Project Objective

Analyze customer types and generate business insights that could help the company identify:
- Top spending customers
- Revenue by country
- Average revenue per customer
- Business insights from customer types

## Dataset Overview

This project uses the **Chinook Database**, a sample dataset that simulates a digital media store.

It contains tables related to:
- Customers
- Invoices & Invoice Lines
- Tracks, Albums, & Artists
- Genres & Media Types
- Employees and Customers’ Countries

 **Key Tables Used in This Project:**
- `Customer`: Contains customer info like name, country, and contact details.
- `Invoice`: Records of purchases made by customers.
- `InvoiceLine`: Line-level details of each invoice.
- `Track`: Details of each song (used indirectly for broader context).

The database mimics real-world business transactions, making it great for practicing SQL queries, sales analysis, and customer behavior insights.

## Tools Used

- **SQL Server** – querying and analysis  
- **Power BI** – dashboard creation and DAX  
- **Microsoft Word** – documentation  
- **GitHub** – project sharing and version control


## Business Insights

-  **Top Spending Customers**  
  Identified top 10 customers with the highest total purchase values.

-  **Revenue by Country**  
  USA leads in revenue, followed by Canada, France, and others.

-  **Average Revenue per Customer**  
  Countries with fewer customers, like Ireland and Czech Republic, still show high spending per user.

-  **Buyer Type Distribution**  
  All customers are repeat buyers, which suggests strong loyalty (based on the demo dataset).


## Power BI Dashboard

The Power BI Dashboard includes:
- Bar chart for **top 10 customers by total spend**
- Column chart for **revenue by country**
- Card for **average revenue per customer**
- Pie Chart for **Buyer Type Distribution (One-Time vs Repeat Buyers)**


![Power BI Dashboard Screenshot](https://github.com/PreciousNwachukwu/chinook-customer-segmentation-sql-powerbi/blob/main/Chinook%20Customer%20Segmentation%20Dashboard.png?raw=true)


## SQL Logic Summary

This section explains the core SQL logic used in the analysis:

- Used `INNER JOIN` to connect the customer, and invoice tables  
-  Applied `GROUP BY`, `CASE`, and `SUM()` to calculate total revenue by customer and country  
- Used `ORDER BY` and `TOP 10` to identify top spenders  
- Structured reusable logic with **CTEs** (Common Table Expressions) for cleaner, more readable sub-queries


## SQL Queries Used

<details>
<summary><strong>Top 10 Highest-Spending Customers</strong></summary>

```sql
SELECT TOP 10 
    Customer.CustomerId,
    Customer.FirstName,
    Customer.LastName,
SUM(Invoice.Total) AS TotalSpent
FROM Customer
INNER JOIN Invoice ON Customer.CustomerId = Invoice.CustomerId
GROUP BY Customer.CustomerId, Customer.FirstName, Customer.LastName
ORDER BY TotalSpent DESC
```
</details>

 <details> 
 <summary><strong>One-Time vs. Repeat Buyers (with Percentage)</strong></summary>

 ```sql
WITH PurchaseCounts AS (
    SELECT Customer.CustomerId, 
           COUNT(*) AS InvoiceCount
    FROM Invoice
    INNER JOIN Customer ON Invoice.CustomerId = Customer.CustomerId
    GROUP BY Customer.CustomerId
),
CustomerType AS (
    SELECT 
        CASE 
            WHEN InvoiceCount = 1 THEN 'One-Time Buyer' 
            ELSE 'Repeat Buyer' 
        END AS BuyerType,
        COUNT(*) AS CountType
    FROM PurchaseCounts
    GROUP BY 
        CASE 
            WHEN InvoiceCount = 1 THEN 'One-Time Buyer' 
            ELSE 'Repeat Buyer' 
        END
)
SELECT 
    BuyerType,
    CountType,
    CAST(CountType * 100.0 / SUM(CountType) OVER () AS DECIMAL(5, 2)) AS Percentage
    FROM CustomerType
```
</details>

<details>
<summary><strong>Customers per Country + Revenue by Country</strong></summary>
  
```sql
SELECT 
    Customer.Country,
    COUNT(DISTINCT Customer.CustomerId) AS NumberOfCustomers,
    SUM(Invoice.Total) AS TotalRevenue
FROM Customer
INNER JOIN Invoice
ON
Customer.CustomerId = Invoice.CustomerId
GROUP BY Customer.Country
ORDER BY TotalRevenue DESC
```
</details>

<details>
<summary><strong>Average Revenue per Customer</strong></summary>
  
```sql
SELECT AVG(CustomerRevenue) AS AverageRevenuePerCustomer
FROM (
    SELECT 
        Customer.CustomerId,
        SUM(Invoice.Total) AS CustomerRevenue
    FROM Customer
    INNER JOIN Invoice ON Customer.CustomerId = Invoice.CustomerId
    GROUP BY Customer.CustomerId) AS RevenuePerCustomer
```
</details>

## Skills Demonstrated

- SQL Query Design & Optimization  
- Data Analysis & Business Insight Extraction  
- Power BI Dashboard Creation
- Data Documentation & Storytelling  
- Collaboration & Peer Review  


## Data Source

- Chinook Music Store Sample Database  
- [Chinook GitHub Repository](https://github.com/lerocha/chinook-database)


## Acknowledgements
Special thanks to Imoleayo, who conducted a parallel Sales & Revenue Analysis. We exchanged feedback and reviewed each other’s SQL logic, which helped elevate the final results.

## Recommendations

- Focus retention efforts on top-spending customers  
- Target high-performing countries for future campaigns  
- Consider loyalty programs to boost revenue per customer  
- Investigate dataset anomalies (e.g., no one-time buyers)  

## Let's Connect

If you like this project or want to collaborate, feel free to reach out or connect with me on [LinkedIn](https://www.linkedin.com/in/precious-nwachukwu-873b432b7/).


## Conclusion

This project blends SQL fluency with real-world business strategy. Though based on a demo dataset, it showcases advanced analytical thinking, data storytelling, and executive-ready visualization core skills for any data-driven role.
From query design to dashboard delivery, it surfaces actionable strategies that optimize customer engagement and drive revenue growth.







