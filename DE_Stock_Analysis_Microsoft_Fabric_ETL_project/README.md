# 📈 Microsoft Fabric Stock Market Analysis ETL Project | Yahoo Finance API | Fabric Data Factory | Fabric Lakehouse (OneLake) | PySpark | Delta Lake | Power BI

## Overview

This project implements an end-to-end Microsoft Fabric Data Engineering pipeline to ingest, process, transform, and analyze stock market data using the Medallion Architecture.

The pipeline uses Microsoft Fabric Data Factory to ingest historical stock data from the Yahoo Finance API into a Fabric Lakehouse. Fabric Spark Notebooks process the data using PySpark and Delta Lake, transforming raw JSON responses into Bronze, Silver, and Gold datasets. The Gold-layer datasets are designed for Power BI dashboards and analytical reporting.

## Technology Stack

- Microsoft Fabric
- Fabric Data Factory
- Yahoo Finance API
- Fabric Lakehouse (OneLake)
- PySpark
- Delta Lake
- Power BI
- GitHub
- Medallion Architecture

---

## Architecture Flow

```text
             Yahoo Finance API
             Stock Market Data
                     |
                     v
          Microsoft Fabric Data Factory
               Data Ingestion
                     |
                     v
             Fabric Lakehouse
              Raw JSON Files
                     |
                     v
             Fabric Spark Notebooks
                PySpark + Delta
                     |
                     v
                Bronze Layer
                 Raw Data
                     |
                     v
                Silver Layer
            Cleansed & Standardized
                     |
                     v
                 Gold Layer
             Analytics-Ready Tables
                     |
                     v
                  Power BI
            Dashboards & Reporting
```

# 1. Data Source

The project uses the Yahoo Finance Chart API to retrieve historical daily stock market data for 15 configured stock tickers.

The API response contains stock price information, timestamps, trading volume, and adjusted closing prices where available.

Source data includes:

- Apple (`AAPL`)
- Advanced Micro Devices (`AMD`)
- Amazon (`AMZN`)
- Salesforce (`CRM`)
- Intel (`INTC`)
- JPMorgan Chase (`JPM`)
- Meta Platforms (`META`)
- Microsoft (`MSFT`)
- Netflix (`NFLX`)
- NVIDIA (`NVDA`)
- Oracle (`ORCL`)
- Palantir Technologies (`PLTR`)
- PayPal (`PYPL`)
- Tesla (`TSLA`)
- Walmart (`WMT`)

The Yahoo Finance API provides the source data, which is ingested into Microsoft Fabric for downstream processing and analysis.

# 2. Microsoft Fabric Data Factory — Data Ingestion

Microsoft Fabric Data Factory is used to orchestrate stock data ingestion from the Yahoo Finance API into the Fabric Lakehouse.

### Responsibilities

- Read the stock ticker configuration using a Lookup activity.
- Iterate through the configured tickers using a ForEach activity.
- Construct ticker-specific API request URLs.
- Use the Copy activity to retrieve stock data.
- Configure the historical date range using start and end timestamps.
- Store the raw JSON responses in the Fabric Lakehouse.

### Data Flow

```
Yahoo Finance API
        |
        v
Lookup Activity
        |
        v
ForEach Activity
        |
        v
Copy_Stock_Ticker_Data
        |
        v
Fabric Lakehouse
```

The ingestion pipeline supports processing multiple stock tickers through a configurable workflow.

# 3. Microsoft Fabric Lakehouse

The Fabric Lakehouse serves as the central storage layer for raw stock data and the processed Delta tables.

The raw API responses are stored in the Lakehouse Files area before being processed by Spark notebooks.

### Example Storage Structure

```
stock_market_analysis_LH
│
├── Files
│   └── Stocks_Raw_Data
│       ├── Apple
│       │   └── AAPL.json
│       ├── Advanced Micro Devices
│       │   └── AMD.json
│       ├── Amazon
│       │   └── AMZN.json
│       └── ... other configured companies
│
└── Tables
    ├── Bronze tables
    ├── Silver tables
    └── Gold tables
```

The Lakehouse separates raw file storage from structured analytical tables and provides the storage foundation for the Medallion Architecture.

# 4. Microsoft Fabric and PySpark

Microsoft Fabric Spark Notebooks are used to process and transform the ingested stock data.

PySpark is used for JSON parsing, data cleaning, consolidation, analytical calculations, and Delta table operations.

### Responsibilities

- Read raw JSON files from the Fabric Lakehouse.
- Parse the Yahoo Finance chart response.
- Extract stock prices, timestamps, volume, and adjusted-close values.
- Transform nested JSON into structured DataFrames.
- Process multiple ticker-level datasets.
- Store the transformed datasets as Delta tables.
- Apply data quality checks and merge logic where implemented.

### Processing Flow

```
Fabric Lakehouse
        |
        v
Fabric Spark Notebooks
        |
        v
       PySpark
        |
        v
   Delta Lake Tables
        |
        v
Bronze → Silver → Gold
```

# 5. Bronze Layer — Raw Data Ingestion

The Bronze layer stores structured stock records extracted from the raw Yahoo Finance JSON responses.

This layer converts the nested API response into a tabular format while retaining the source-level stock information.

### Responsibilities

- Read raw JSON files from the Lakehouse.
- Parse the Yahoo Finance chart response.
- Extract timestamps and quote arrays.
- Extract open, high, low, close, and volume values.
- Extract adjusted-close values when available.
- Convert Unix timestamps into trade date/time values.
- Retain ticker information and source metadata where implemented.
- Store the processed records as Delta tables.

### Example Tables

- `brz_aapl_stock`
- `brz_amd_stock`
- `brz_amzn_stock`
- `brz_msft_stock`
- `brz_nvda_stock`
- `brz_wmt_stock`

The project follows a ticker-level Bronze table naming pattern for the configured stocks.

### Data Flow

```
Lakehouse Raw JSON Files
          |
          v
    PySpark Processing
          |
          v
    Parse Stock Records
          |
          v
    Bronze Delta Tables
```

# 6. Silver Layer — Data Cleaning and Transformation

The Silver layer consolidates the ticker-level Bronze tables into a standardized stock prices dataset.

PySpark transformations are applied to clean, validate, and prepare the data for analytical processing.

### Responsibilities

- Read eligible records from the Bronze tables.
- Combine the stock datasets using schema-compatible unions.
- Standardize column names and data types.
- Handle missing ticker, timestamp, and closing-price values.
- Filter invalid closing prices according to the implemented rules.
- Maintain consistent stock and timestamp keys.
- Prepare the consolidated dataset for Gold-layer transformations.
- Apply Delta merge logic where implemented.

### Example Transformations

- Filter records with null ticker or timestamp values.
- Exclude records with missing or non-positive closing prices.
- Standardize numeric and timestamp fields.
- Combine data from all 15 stock tickers.
- Check duplicate `(ticker, timestamp)` keys.
- Prepare a consistent stock prices dataset for analytical calculations.

### Data Flow

```
Bronze Tables
      |
      v
Data Consolidation
      |
      v
Data Cleaning
      |
      v
Data Quality Validation
      |
      v
Silver Delta Table
```

### Silver Table

`silver.slv_stock_prices`

The Silver layer provides a consolidated dataset for calculating stock performance, return-based sentiment, and forecast baselines.

# 7. Gold Layer — Analytics-Ready Data

The Gold layer contains analytical datasets derived from the consolidated Silver stock prices.

PySpark transformations calculate stock performance indicators and prepare datasets for Power BI reporting.

### Responsibilities

- Calculate daily stock returns.
- Calculate seven-day and thirty-day moving averages.
- Calculate thirty-day rolling volatility.
- Calculate cumulative returns.
- Generate rule-based stock sentiment labels.
- Create a seven-trading-day last-close forecast baseline.
- Create a date dimension for analytical reporting.
- Store the analytical datasets as Delta tables.

### Gold Tables

- `gold.gld_daily_stock_performance`
- `gold.gld_stock_sentiment_analysis`
- `gold.gld_stock_price_forecast`
- `gold.gld_dim_date`

### Daily Stock Performance

The daily stock performance table includes indicators such as:

- Daily return
- Seven-day moving average
- Thirty-day moving average
- Thirty-day rolling volatility
- Cumulative return

These metrics support analysis of historical stock performance and price movements.

### Stock Sentiment Analysis

The sentiment table classifies stock price movements using rule-based return logic.

Possible classifications include:

- Positive
- Negative
- Neutral

This is a return-based classification rather than news sentiment analysis or a machine-learning model.

### Stock Price Forecast

The forecast table uses the latest available closing price as a baseline for a seven-trading-day forecast.

This is a simple baseline for demonstrating analytical data preparation, not a trained predictive model or investment recommendation.

### Date Dimension

The date dimension provides a common date reference for reporting across the Gold datasets.

### Data Flow

```
Silver Stock Prices
        |
        v
   Gold Transformations
        |
        +-------------------------+
        |            |            |
        v            v            v
 Daily Stock     Sentiment     Price Forecast
 Performance     Analysis      (7-Day Baseline)
        |            |            |
        +------------+------------+
                     |
                     v
                Gold Delta Tables
                     |
                     v
                 Power BI Reports
```

# 8. Fabric Lakehouse — Data Organization

The Fabric Lakehouse organizes the raw files and the structured Delta tables used throughout the pipeline.

### Example Logical Structure

```
stock_market_analysis_LH
│
├── Files
│   └── Stocks_Raw_Data
│
└── Tables
    ├── Bronze
    │   ├── brz_aapl_stock
    │   ├── brz_amd_stock
    │   ├── brz_amzn_stock
    │   └── ... other ticker tables
    │
    ├── Silver
    │   └── slv_stock_prices
    │
    └── Gold
        ├── gld_daily_stock_performance
        ├── gld_stock_sentiment_analysis
        ├── gld_stock_price_forecast
        └── gld_dim_date
```

The logical structure above illustrates the project layers. Actual schema and table locations depend on the Lakehouse and notebook configuration.

### Lakehouse Capabilities Used

- Raw JSON file storage.
- Structured Delta table storage.
- Separation of raw and transformed datasets.
- Storage for Bronze, Silver, and Gold data.
- Access to processed data for downstream reporting.

# 9. Power BI — Data Visualization

Power BI is used to visualize the Gold-layer stock datasets and present historical stock market insights through dashboards.

The Gold tables provide the analytical data required for reporting.

### Dashboard Objectives

- Analyze historical stock price movements.
- Compare daily returns across tickers.
- Examine moving averages and volatility.
- Analyze positive, negative, and neutral return classifications.
- Visualize the seven-trading-day forecast baseline.
- Compare performance across the configured stocks.

### Example Visualizations

- Line charts for historical stock closing prices.
- Line charts for moving averages.
- Bar charts for daily return comparisons.
- Charts for cumulative returns and volatility.
- Sentiment distribution charts.
- Historical price versus baseline forecast charts.
- Ticker and date filters.

### Reporting Flow

```
Gold Delta Tables
        |
        v
Power BI Data Connection
        |
        v
     Data Modeling
        |
        v
Interactive Dashboards
```

The semantic model and reports can be configured using the supported Power BI connection to the Fabric data.

# 10. End-to-End Pipeline

The complete pipeline follows these stages:

```
Yahoo Finance API
          |
          v
Microsoft Fabric Data Factory
          |
          v
Fabric Lakehouse Raw JSON Files
          |
          v
Fabric Spark Notebooks
          |
          v
       Bronze Layer
        Raw Data
          |
          v
       Silver Layer
   Cleansed & Standardized
          |
          v
        Gold Layer
   Analytics-Ready Tables
          |
          v
         Power BI
   Dashboards & Stock Analysis
```

Each stage has a separate responsibility, separating ingestion, transformation, analytical processing, and reporting.

# 11. Data Layer Responsibilities

| Layer               | Purpose            | Main Operations                                            |
| ------------------- | ------------------ | ---------------------------------------------------------- |
| Yahoo Finance API   | Source data        | Retrieve historical daily stock data                       |
| Fabric Data Factory | Data ingestion     | Lookup, ForEach, and Copy activities                       |
| Lakehouse Files     | Raw data storage   | Store stock JSON responses                                 |
| Bronze              | Raw processed data | Parse JSON and create ticker-level Delta tables            |
| Silver              | Trusted data       | Consolidation, cleaning, validation, and standardization   |
| Gold                | Analytical data    | Stock performance, sentiment, forecast, and date dimension |
| Power BI            | Reporting          | Data modeling and visualization                            |

# 12. Key Design Principles

### Separation of Responsibilities

Fabric Data Factory handles ingestion, Spark Notebooks handle transformations, the Lakehouse stores the data, and Power BI supports analytical reporting.

### Medallion Architecture

Data progresses through Bronze, Silver, and Gold layers, from structured source records to cleansed data and analytical datasets.

### Data Quality

Silver transformations validate required fields, standardize data types, and filter invalid stock price records.

### Reusability

The consolidated Silver table serves as a common input for multiple Gold analytical datasets.

### Incremental Processing

Delta merge logic can support repeatable updates when the source data is deduplicated and the stock key is handled consistently.

### Analytics Readiness

Gold tables provide structured datasets for historical stock performance analysis and Power BI reporting.

# 13. Repository Structure

The project repository contains the source files and documentation associated with the Microsoft Fabric stock analysis pipeline.

```
DE_Stock_Analysis_MicrosoftFabric_ETL_project/
│
├── README.md
├── Fabric Data Factory pipeline assets or documentation
├── Bronze notebook / source file
├── Silver notebook / source file
├── Gold notebook / source file
└── Other project assets, if included
```

The filenames above describe the expected roles of the project assets. Actual filenames should match the files committed to the repository.

# 14. Getting Started

## Prerequisites

- Microsoft Fabric workspace access.
- Access to the Fabric Lakehouse.
- Permission to run Fabric Spark Notebooks.
- Access to Fabric Data Factory.
- Access to the Yahoo Finance API endpoint from the configured pipeline.
- Power BI access for dashboard creation.

## Execution Workflow

1. Configure stock tickers: Review the ticker list and company names used by the ingestion pipeline.
2. Configure Fabric Data Factory: Verify the Lookup, ForEach, Copy activity, dynamic API URL, and date range.
3. Verify Lakehouse storage: Confirm the destination paths for the raw JSON files.
4. Execute ingestion: Run the pipeline and verify that the configured ticker files are created.
5. Execute Bronze processing: Parse the JSON responses and create the ticker-level Bronze tables.
6. Execute Silver processing: Consolidate the Bronze tables and apply the required cleaning and validation logic.
7. Execute Gold processing: Generate the stock performance, sentiment, forecast, and date-dimension tables.
8. Connect Power BI: Connect to the Gold datasets and create the required analytical reports.

# 15. Project Outcome

This project demonstrates an end-to-end Microsoft Fabric data engineering workflow for ingesting, transforming, and analyzing stock market data.

It showcases practical implementation of:

- API-based data ingestion using Fabric Data Factory.
- Raw data storage using a Fabric Lakehouse.
- Distributed data processing using PySpark.
- Medallion Architecture using Delta Lake.
- Data cleaning and validation.
- Delta table processing and merge patterns.
- Stock performance calculations using window functions.
- Analytical dataset preparation for Power BI.

The final outcome is a structured stock market data pipeline that transforms Yahoo Finance API responses into analytical datasets for historical stock performance analysis and reporting.

# Medallion Architecture Diagram

<img src="https://github.com/2003ohith/DE_Stock_Analysis_MicrosoftFabric_ETL_project/blob/main/Microsoft_Fabric_Stock_Analysis_Data_Engineering_Medallion_Architecture.png" alt="Microsoft Fabric Stock Analysis Project Architecture" width="900">
