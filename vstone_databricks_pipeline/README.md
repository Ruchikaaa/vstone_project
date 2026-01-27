# **VStone: Secure Car Sales Analytics Pipeline**

### **End-to-End Medallion Architecture with Unity Catalog Governance**

## **📌 Project Overview**

VStone is a comprehensive data engineering solution built on Databricks to process, transform, and secure large-scale car sales data. The project handles over **500,000 sales records** and integrates a dataset of **1,500+ geographic locations** to provide regionalized market insights.

The core of this project is **Identity-Aware Governance**. Using Unity Catalog, the pipeline ensures that sensitive financial data is masked and regional data is filtered based on the user's organizational role (Managers vs. Agents).

---

## **🏗 Architecture**

The project follows the **Medallion Architecture** to ensure data quality and lineage:

1. **Bronze Layer**: Raw ingestion of CSV files (Sales and Geography) into Delta format.  
2. **Silver Layer**: Data cleaning, schema enforcement, and deduplication.  
3. **Gold Layer**: A unified, physical Fact Table (`fact_car_sales`) joined via business logic and hardened with Row-Level Security (RLS) and Column-Level Masking (CLS).

---

## **🛠 Tech Stack**

* **Platform**: Databricks (Lakehouse Architecture)  
* **Storage**: Delta Lake  
* **Governance**: Unity Catalog  
* **Language**: Spark SQL / Python  
* **Warehouse**: Serverless SQL Warehouse  
* **Visualization**: Databricks SQL Dashboards

---

## **🔐 Data Governance (The Security Model)**

This project implements **Fine-Grained Access Control (FGAC)** directly at the storage level.

### **1\. Row-Level Security (RLS)**

Filters rows based on the user's regional assignment.

* **Managers**: View all global records.  
* **Agents**: Restricted to specific cities (e.g., *Кемерово*) via the `row_filter_by_group` function.

### **2\. Column-Level Masking (CLS)**

Protects sensitive financial information.

* **Managers**: View the actual `cost` of the vehicle.  
* **Agents**: The `cost` column is dynamically masked to `0.0` using the `cost_mask_by_group` function.

---

## **📊 Business Glossary & KPIs**

| KPI | Calculation | Purpose |
| :---- | :---- | :---- |
| **Total Regional Revenue** | SUM(cost) | Measures financial market scale (Manager-only). |
| **Total Units Sold** | COUNT(id) | Measures inventory turnover and volume. |
| **Avg. Vehicle Mileage** | AVG(probeg) | Determines inventory quality and age. |
| **Market Share %** | (City Count / Total) \* 100 | Identifies dominant regional branches. |

## **🚀 Installation & Setup**

1. **Catalog Setup**: Run the environment setup script to create the `vstone_project` catalog and `db_project` schema.  
2. **Ingestion**: Upload `final_geografic.csv` and your sales data to the volume.  
3. **Pipeline Execution**: Run the SQL notebooks in sequence (Bronze \-\> Silver \-\> Gold).  
4. **Governance Activation**: Run the `ALTER TABLE` commands to bind the security functions to the physical Gold table.  
5. **Dashboarding**: Import the provided SQL queries into the Databricks SQL Editor to generate the 3-page reporting suite.

---

## **📈 Dashboard Highlights**

* **Operational Page**: High-level volume and mileage metrics for all staff.  
* **Financial Page**: Revenue and profit analysis, restricted to the `vstone_managers` group.  
* **Technical Audit**: Pipeline freshness, sync timestamps, and currency distribution.

---

## **💡 Key Engineering Solutions**

* **Modulo Mapping**: Solved the "Missing Key" challenge by developing a mathematical distribution logic to link transaction IDs to geographic identifiers.  
* **Physical Materialization**: Overcame the `EXPECT_TABLE_NOT_VIEW` limitation by materializing data into physical Delta tables, ensuring that Unity Catalog security policies are enforced at the hardware level.

---

## **👥 Authors**

* **Associate Data Engineer**: Ruchika Mhetre  
* **Project Context**: 10-Day Data Engineering Sprint (VStone)