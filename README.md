# Simple Sales Data Warehouse: End-to-End ELT Pipeline

## Project Overview
This is my first milestone project in Data Engineering. It demonstrates a complete workflow of extracting raw sales data, loading it into a staging environment, and transforming it into a structured **Star Schema** within SQL Server. 

The project follows the **ELT (Extract, Load, Transform)** pattern, leveraging the power of T-SQL for data processing.

## Architecture
The project is built on a **Star Schema** design to optimize analytical queries:
* **Fact Table:** `Fact_Sales` (Contains metrics like Quantity and calculated TotalSales).
* **Dimension Tables:** `Dim_Product`, `Dim_Customer` (Contains descriptive attributes).

## Tech Stack
* **Orchestration:** SQL Server Integration Services (SSIS).
* **Database Engine:** SQL Server (T-SQL).
* **Data Source:** CSV Files.

## Pipeline Workflow
1. **Extraction & Loading (SSIS):** - Automates the data flow from source files.
   - Uses a **Staging Area** strategy.
   - Includes `TRUNCATE` tasks to ensure data freshness and prevent duplication.
2. **Transformation (T-SQL):**
   - Implemented an ELT approach.
   - Complex business logic (e.g., calculating `TotalSales`) is handled via SQL scripts during the load from Staging to the Fact table.
3. **Data Integrity:**
   - Managed Surrogate Keys and Foreign Key relationships.
   - Ensured data mapping accuracy between source and destination.

## Key Insights (Sample Query)
I included a performance analysis query that ranks products by revenue. This validates the integrity of the joins and the accuracy of the transformations.

```sql
-- Top-Selling Products Performance Analysis
SELECT 
    p.ProductID,
    COUNT(f.SalesKey) AS TotalOrders,
    SUM(f.TotalSales) AS Revenue
FROM Fact_Sales f
JOIN Dim_Product p ON f.ProductKey = p.ProductKey
GROUP BY p.ProductID
ORDER BY Revenue DESC;
