# 👨‍💻 Data Engineering Portfolio

**Author:** Ohith Prasanna Venugopal  
**Role:** QA Engineer (BI & ETL Tester) | Aspiring Data Engineer *(Microsoft Fabric/Azure)*

---

## 📘 Overview

This repository showcases end-to-end **Data Engineering projects** built using **Microsoft Azure and Microsoft Fabric**, leveraging PySpark, SQL, Delta Lake, cloud data storage, and analytics tools.

The projects demonstrate data ingestion, ETL/ELT pipeline development, data cleansing, lakehouse architecture, data governance, and analytics-ready data preparation. Where applicable, projects follow the **Medallion Architecture (Bronze → Silver → Gold)** to transform raw data into curated datasets for reporting and analysis.

### Projects Included

1. **AdventureWorks Azure ETL Project** — An end-to-end Azure data engineering pipeline for ingesting and transforming AdventureWorks data using Azure Data Factory, ADLS Gen2, Databricks, PySpark, Delta Lake, and Unity Catalog.
2. **E-commerce Azure ETL Project** — An Azure-based data engineering project focused on ingesting, processing, and transforming 32M+ transaction records into a Star Schema for customer purchase and order analytics.
3. **NYC Yellow Taxi Trip Azure ETL Project** — A cloud data engineering pipeline for processing NYC Yellow Taxi trip data using Azure data services, PySpark, and lakehouse technologies.

---

## 🏗️ Medallion Architecture Overview

The Medallion Architecture organizes data into three logical layers to support reliable and scalable data processing.

- **Bronze:** Raw data ingestion from source systems into cloud storage.
- **Silver:** Data cleansing, validation, standardization, and transformation.
- **Gold:** Curated datasets and analytical models prepared for reporting and business insights.

**Typical Data Flow:**

`Source Data → Data Ingestion → Bronze → Silver → Gold → Analytics & Reporting`

---

## 🧩 Technologies Used

### ☁️ Microsoft Azure Data Engineering

- **Azure Data Factory (ADF)** — Data ingestion, pipeline orchestration, and scheduled workflows.
- **Azure Data Lake Storage Gen2 (ADLS Gen2)** — Scalable cloud storage for raw and processed datasets.
- **Azure Databricks** — Distributed data processing and transformation.
- **PySpark** — DataFrame transformations, joins, aggregations, and data cleansing.
- **Delta Lake** — ACID transactions, schema handling, and reliable lakehouse storage.
- **Unity Catalog** — Data governance, access control, and catalog management.
- **Parquet** — Columnar storage for analytical workloads.
- **Azure Synapse Analytics** — Data serving and analytical querying where implemented.
- **SQL** — Data validation, querying, and transformation logic.

### 🔷 Microsoft Fabric Data Engineering & Analytics

- **Microsoft Fabric Lakehouse** — Unified data storage and lakehouse architecture.
- **OneLake** — Centralized data storage across Fabric workloads.
- **Fabric Data Pipelines** — Workflow orchestration and scheduled data processing.
- **Fabric Notebooks** — PySpark-based data ingestion and transformation.
- **Dataflow Gen2** — Low-code data ingestion and transformation.
- **Delta Lake** — Reliable storage and transactional data processing.
- **Power BI** — Interactive dashboards, reporting, and visualization.
- **DAX** — Measures and analytical calculations.
- **Semantic Models** — Business-friendly analytical modeling, including Star Schema design.
- **Python and APIs** — Data extraction, API integration, and data preparation where required.

### 🛠️ Core Data Engineering Concepts

- ETL and ELT pipeline development
- Medallion Architecture
- Batch data ingestion and orchestration
- PySpark DataFrame API and Spark SQL
- Data cleansing and schema enforcement
- Incremental data processing where implemented
- Delta Lake and Parquet storage
- Data governance and catalog management
- Dimensional modeling and Star Schema
- Analytics-ready data preparation

---

## 🚀 Portfolio Projects

### 📈 1. Real-Time Stock Market Data Pipeline *(Latest Project)*

**Objective:**  
Build a fully automated **daily stock market analytics pipeline** on Microsoft Fabric that ingests historical and incremental stock market data from Yahoo Finance, processes it using PySpark, and generates analytics-ready datasets for Power BI reporting.

**Key Highlights:**

- **Incremental Data Processing:** Implemented Delta Lake `MERGE` (Upsert) operations to insert new stock records and update existing records, supporting incremental data processing without requiring full reloads.
- **Advanced PySpark Transformations:** Applied PySpark transformations and Window functions to calculate daily stock returns, 7-day and 30-day moving averages, volatility, and cumulative returns for time-series analysis.
- **Medallion Lakehouse Architecture:** Implemented a Bronze, Silver, and Gold architecture in Microsoft Fabric Lakehouse to organize raw Yahoo Finance JSON data, cleansed stock prices, and curated analytical datasets.
- **Data Quality & Schema Enforcement:** Applied explicit data type casting, null handling, and data validation to maintain consistent stock price datasets across processing layers.
- **Pipeline Orchestration:** Configured a Microsoft Fabric Data Factory pipeline using Lookup, ForEach, and Copy Data activities to automate ingestion across multiple stock tickers.
- **Analytics & Forecasting:** Developed Gold-layer datasets for daily stock performance, return-based sentiment classification, and baseline 7-trading-day price forecasting.
- **Dimensional Modeling:** Designed a **Star Schema** with `FactStocks` (Gold) and `DimCompany`/`DimDate` to support time-series analysis, stock performance reporting, and cross-company comparisons.
- **Power BI Integration:** Prepared curated Gold-layer datasets for Power BI dashboards to analyze stock price trends, returns, moving averages, volatility, and sentiment across multiple companies.

**Technologies:** Microsoft Fabric, Fabric Data Factory, Fabric Lakehouse, PySpark, Spark Notebooks, Delta Lake, Yahoo Finance API, JSON, SQL, Medallion Architecture, Incremental ETL/ELT, Delta MERGE (Upsert), Window Functions, and Power BI.

📂 **[View Stock Market Analysis – Microsoft Fabric ETL Project](https://github.com/2003ohith/DE_Stock_Analysis_MicrosoftFabric_ETL_project)**

---

### 🚕 2. NYC Yellow Taxi Trip Azure ETL Project *(Advanced Project)*

**Objective:**

Build an end-to-end Azure Data Engineering pipeline to ingest and transform **32M+ NYC Yellow Taxi records**, implementing a Lakehouse architecture that produces structured, analytics-ready datasets for trip analysis and reporting.

**Key Highlights:**

- **End-to-End Lakehouse Architecture:** Implemented a scalable **Medallion Architecture (Bronze/Silver/Gold)** on **Azure Data Lake Storage Gen2**, ingesting and processing **32M+ NYC Taxi records** using Azure Data Factory and PySpark.
- **Data Engineering & Transformation:** Developed ETL pipelines to cleanse, transform, and validate taxi trip data, applying schema handling and storing processed datasets in **Delta Lake** for reliable downstream analytics.
- **Analytics Enablement:** Built curated **Gold-layer datasets** to support analysis of taxi trip patterns, revenue, and trip distribution, enabling efficient querying and analytics-ready data for reporting.
- **Incremental Loading:** Processed NYC Yellow Taxi data using batch processing through Azure Data Factory and PySpark, with Delta Lake **MERGE (UPSERT)** supporting reliable data storage and updates.
- **Slowly Changing Dimensions (SCD):** Applied SCD Type 1  for overwriting updated records 

**Technologies:** Azure Data Factory, ADLS Gen2, Azure Databricks, PySpark, Delta Lake, Parquet, Unity Catalog, and SQL where implemented.

📂 **[View NYC Yellow Taxi Trip - Azure ETL Project](https://github.com/2003ohith/DE_NYC-Taxi-Trip_Azure_ETL_Project)**

---

### 🛍️ 3. E-commerce Azure ETL Project

**Objective:**

Develop an Azure-based data engineering workflow for ingesting, processing, and transforming e-commerce data into structured datasets suitable for downstream analytics.

**Key Highlights:**

- **Data Ingestion:** Establish a workflow for bringing source data into the data lake.
- **Cloud Storage:** Use Azure data storage services to organize datasets.
- **Data Transformation:** Apply data cleansing, standardization, and transformation logic.
- **Lakehouse Processing:** Structure data for efficient analytical access using the storage formats and processing services implemented in the project.
- **Analytics Preparation:** Prepare processed data for downstream reporting and business analysis.
- **Incremental Loading:** Processed Ecommerce data using Stream processing through Azure Data Factory and PySpark, with Delta Lake **MERGE (UPSERT)** supporting reliable data storage and updates.
- **Slowly Changing Dimensions (SCD):** Applied SCD Type 1  for overwriting updated records 

**Technologies:** Azure Data Factory, ADLS Gen2, Azure Databricks, PySpark, Delta Lake, and SQL where implemented.

📂 **[View E-commerce Azure ETL Project](https://github.com/2003ohith/DE_Ecommerce_Azure_ETL_project)**

---

### 🏭 4. AdventureWorks Azure ETL Project

**Objective:**

Build an end-to-end Azure Data Engineering pipeline that ingests AdventureWorks datasets, processes the data through a Medallion Architecture, and prepares curated datasets for downstream analytics.

**Key Highlights:**

- **Data Ingestion:** Ingest source CSV datasets using GitHub Raw HTTPS and Azure Data Factory.
- **Cloud Storage:** Organize raw and processed data in Azure Data Lake Storage Gen2.
- **Data Transformation:** Use Azure Databricks and PySpark to clean, standardize, and transform datasets.
- **Medallion Architecture:** Separate raw, cleansed, and curated data into Bronze, Silver, and Gold layers.
- **Data Governance:** Use Unity Catalog to organize and govern data assets.
- **Analytics Preparation:** Prepare curated datasets for downstream analytical querying and reporting.

**Technologies:** Azure Data Factory, ADLS Gen2, Azure Databricks, PySpark, Delta Lake, Parquet, Unity Catalog, Azure Synapse Analytics, and Power BI where implemented.

📂 **[View AdventureWorks Azure ETL Project](https://github.com/2003ohith/DE_AdventureWorks_Azure_ETL_project)**

---

## 🧠 Concepts Demonstrated

✅ ETL and ELT Pipeline Development  
✅ Medallion Architecture (Bronze / Silver / Gold)  
✅ Azure Data Factory Pipeline Orchestration  
✅ Azure Data Lake Storage Gen2  
✅ PySpark DataFrame Transformations and Spark SQL  
✅ Data Cleansing, Type Casting, and Schema Handling  
✅ Delta Lake and Parquet Storage  
✅ Unity Catalog and Data Governance  
✅ Microsoft Fabric Lakehouse and OneLake  
✅ Dataflow Gen2 and Fabric Data Pipelines  
✅ Dimensional Modeling and Star Schema  
✅ Power BI Reporting and Analytics Integration  

---

## 👤 Connect with Me

- **LinkedIn:** [Ohith Prasanna](https://www.linkedin.com/in/ohith-prasanna/)

