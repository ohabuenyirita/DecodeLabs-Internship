# Project 3: Relational Database Querying & Data Analysis (SQL)

> **DecodeLabs Data Analytics Internship — Task 3**  
> *Querying, filtering, aggregating, and analyzing transactional sales data using Microsoft SQL Server Management Studio (SSMS).*

---

## Executive Summary

In this third project, the cleaned transactional sales dataset from Task 1 was ingested into **Microsoft SQL Server** under the database `DecodeLabs_Project3`. Using **T-SQL** in **SQL Server Management Studio (SSMS)**, structured database queries were constructed to retrieve transactional slices, evaluate conditional revenue thresholds, aggregate sales performance by product and payment method, and filter aggregated business buckets.

This project demonstrates proficiency in standard database querying techniques, including data selection, filtering, multi-metric grouping, and conditional aggregation.

---

## Technical Highlights & Key Query Constructs

* **Database Engine:** Microsoft SQL Server (SSMS).
* **Database Name:** `DecodeLabs_Project3`
* **Target Table:** `[dbo].[Cleaned Dataset for Data Analytics]`
* **Data Inspection:** Executed `SELECT TOP 10` queries to examine column schema and inspect row-level transactional records.
* **Conditional Filtering:** Applied multi-condition `WHERE` logic combining order fulfillment states (`OrderStatus = 'Shipped'`) with revenue boundaries (`TotalPrice >= 200`).
* **Multi-Metric Aggregations:** Utilized `GROUP BY` alongside `COUNT()`, `SUM()`, and `AVG()` functions to evaluate transactional volume and financial metrics across product and payment channels.
* **Bucket-Level Filtering:** Implemented `HAVING` clauses to isolate high-performing aggregated groups based on total revenue thresholds (`SUM(TotalPrice) > 1000`).

---

## SQL Queries & Analytical Workflow

### 1. Database Selection & Context Initialization

```sql
USE DecodeLabs_Project3;
GO
```

### 2. Initial Data Inspection & Column Selection
```sql
-- View top 10 records across all table features
SELECT TOP 10 *
FROM [dbo].[Cleaned Dataset for Data Analytics];

-- Project specific column subsets for targeted analysis
-- Using Select function view the first 10 rows
SELECT TOP 10
    OrderID,
    Date,
    Product,
    Quantity,
    TotalPrice,
    OrderStatus
FROM [dbo].[Cleaned Dataset for Data Analytics];
```
* **Purpose:** Inspect schema architecture, data types, and primary key formatting before running analytical aggregations.

### 3. Targeted Order Filtering (WHERE Clause)
```sql
-- Isolate high-value shipped transactions
-- Using where function
SELECT
    OrderID,
    CustomerID,
    Product,
    TotalPrice,
    OrderStatus
FROM [dbo].[Cleaned Dataset for Data Analytics]
WHERE OrderStatus = 'Shipped' AND TotalPrice >= 200
ORDER BY TotalPrice DESC;
```
* **Purpose:** Filter row-level records to identify delivered/shipped orders generating $200 or higher, sorted by revenue in descending order.

### 4. Product Performance Aggregation (GROUP BY, SUM, AVG, COUNT)
```sql
-- Product performance breakdown
SELECT
    Product,
    COUNT(OrderID) AS TotalOrders,        -- Measures scale and volume
    SUM(TotalPrice) AS TotalRevenue,      -- Tracks financial health
    AVG(TotalPrice) AS AverageOrderValue  -- Identifies norms and typicality
FROM [dbo].[Cleaned Dataset for Data Analytics]
GROUP BY Product;
```
* **Purpose:** Aggregate sales metrics per product line item to measure transaction volume, total revenue, and average spend per order.

### 5. Payment Channel Financial Metrics

```SQL
-- Financial summary across payment channels
SELECT
    PaymentMethod,
    COUNT(OrderID) AS TransactionCount,
    SUM(TotalPrice) AS RevenueByMethod,
    AVG(TotalPrice) AS AvgSpentPerTransaction
FROM [dbo].[Cleaned Dataset for Data Analytics]
GROUP BY PaymentMethod
ORDER BY TransactionCount DESC;
```
* **Purpose:** Evaluate customer payment behavior and total revenue distribution across payment channels.

### 6. Aggregated Group Filtering (HAVING Clause)

```sql
-- Filter aggregated buckets based on revenue thresholds
SELECT
    Product,
    COUNT(OrderID) AS TotalOrders,
    SUM(TotalPrice) AS TotalRevenue
FROM [dbo].[Cleaned Dataset for Data Analytics]
GROUP BY Product
HAVING SUM(TotalPrice) > 1000  -- Filters the grouped buckets (not individual rows)
ORDER BY TotalRevenue DESC;
```
* **Purpose:** Isolate top-tier product categories whose cumulative revenue exceeds $1,000.

  ### Query Output Preview

  | OrderID | CustomerID | Product | TotalPrice ($) | OrderStatus|
  | :--- | :--- | :--- | :--- | :--- |
  | ORD200107 | C16775 | Printer | $3,353.75 | Shipped |
  | ORD200463 | C25276 | Laptop | $3,313.90 | Shipped |
  | ORD200492 | C39074 | Laptop | $3,032.60 | Shipped |
  | ORD200000 | C72649 | Monitor | $2,853.10 | Shipped |

  ---
  ### Repository Files

* **`README.md`** — Project documentation and business insights (this file).
* **`project_3_queries.sql`** — Complete T-SQL script executed in SSMS.

---

### How to Run in SSMS

1. Launch **Microsoft SQL Server Management Studio (SSMS)** and connect to your SQL Server instance.
2. In the **Object Explorer**, create or verify the existence of the database context:
```sql
CREATE DATABASE DecodeLabs_Project3;
GO
USE DecodeLabs_Project3;
GO
3. In SSMS, go to File > Open > File... (or press Ctrl + O) and select project_3_queries.sql.
4. Ensure your active database context is set to DecodeLabs_Project3.
5. Execute the script by clicking Execute or pressing F5.

