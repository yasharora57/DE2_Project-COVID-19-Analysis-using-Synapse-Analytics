# NYC Green Taxi Trip Data Analysis with Azure Synapse Analytics

## Overview

This project focuses on analyzing New York City Green Taxi trip data from January 2020 to June 2021 using Azure Synapse Analytics. The data is sourced from the [TLC Trip Record Data](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page) and includes various files such as Taxi Zone, Calendar, Trip Type, Rate Code, Payment Type, Vendor, and the main Trip Data file. The project follows a **medallion architecture** (bronze, silver, gold layers) to process and analyze the data, leveraging Azure Synapse's **serverless SQL pool** and **dedicated SQL pool** for data discovery, transformation, and reporting.

The project aims to address three key reporting requirements:
1. **Taxi Demand Analysis**
2. **Credit Card Campaign Analysis**
3. **Operational Reporting**

---

## Data Sources

The following files are used in this project, stored in the **raw container** of the Azure Data Lake Storage (ADLS) linked to the Synapse workspace:

- **Taxi Zone** (CSV)
- **Calendar** (CSV)
- **Trip Type** (TSV)
- **Rate Code** (JSON)
- **Payment Type** (JSON)
- **Vendor** (CSV)
- **Trip Data** (partitioned by year and month, available in CSV, Parquet, and Delta formats)

---

## Architecture

The project follows a **medallion architecture** with three layers:

1. **Bronze Layer**: Raw data is ingested into external tables and views.
2. **Silver Layer**: Data is cleaned, transformed, and stored in a structured format.
3. **Gold Layer**: Data is aggregated and optimized for reporting and analysis.

---

## Steps

### 1. Data Discovery
- **Folder**: `discovery`
- Conducted exploratory data analysis (EDA) using the **serverless SQL pool**.
- Identified duplicates, missing values, and invalid data.
- Joined data from multiple files (e.g., Taxi Zone and Trip Data) to evaluate trips by borough.
- Used **OPENROWSET** for partition pruning to optimize query costs.
- Key tasks:
  - Checked for location duplicates in Taxi Zone data.
  - Evaluated data quality issues in the Trip Data's `total_amount` column.
  - Joined Trip Data, Taxi Zone, and Payment Type to analyze payment behavior by borough.

---

### 2. Logical Data Warehouse (LDW) Setup
- **Folder**: `lwh`
- Created a database `nyc_taxi_ldw` with three schemas: **bronze**, **silver**, and **gold**.
- Defined external data sources and file formats for CSV, TSV, Parquet, and Delta files.
- Created external tables and views in the **bronze layer**.
- Used **CETAS (Create External Table As Select)** to transform and store data in the **silver layer**.
- Partitioned data in the silver layer using stored procedures for efficient querying.

---

### 3. Gold Layer Aggregation
- **Folder**: `dwh`
- Created stored procedures to aggregate data for the **gold layer**.
- Partitioned data by year and month for efficient reporting.
- Key aggregations:
  - Trips by payment type (credit card vs. cash).
  - Trips by borough and day of the week.
  - Trip distance, duration, and fare amounts.

---

### 4. Dedicated SQL Pool and Power BI Integration
- Created an internal table in the **dedicated SQL pool** from the gold layer's external table.
- Connected the internal table to **Power BI** for visualization and analysis.
- Key visualizations:
  - **Credit Card Campaign Analysis**:
    - Payment type by borough (bar chart).
    - Payment type by day of the week (line chart).
    - Payment type by week (line chart).
  - **Taxi Demand Analysis**:
    - Demand by day of the week (bar chart).
    - Demand by borough (bar chart).
    - Trip type by month (line chart).

---

## Key Features

- **Cost Optimization**: Used **OPENROWSET** for partition pruning to reduce query costs in the serverless SQL pool.
- **Scalability**: Leveraged Synapse's serverless and dedicated SQL pools for scalable data processing.
- **Automation**: Orchestrated the entire workflow using **Synapse Pipelines**.
- **Pre-Aggregated Reporting**: Aggregated data in the gold layer for efficient reporting and analysis.

---

## Reporting Requirements

### 1. Taxi Demand Analysis
- Analyzed taxi demand by borough and day of the week.
- Evaluated trip types (street-hail vs. dispatch) over time.

### 2. Credit Card Campaign Analysis
- Assessed payment behavior by borough and day of the week.
- Identified trends in credit card and cash payments.

### 3. Operational Reporting
- Provided insights into trip distances, durations, and fare amounts.
- Enabled efficient reporting through pre-aggregated data.

---

## Tools and Technologies

- **Azure Synapse Analytics**: Serverless SQL pool, dedicated SQL pool, Synapse Pipelines.
- **Azure Data Lake Storage (ADLS)**: Raw, silver, and gold containers.
- **Power BI**: Data visualization and reporting.
- **SQL**: Data transformation and aggregation.

---

## How to Use

1. **Clone the repository**:
   ```bash
   git clone https://github.com/yasharora57/DE2_Project-NYC-Taxi-Trip-Analysis-using-Synapse-Analytics.git
   ```
2. **Explore the folders**:
   - `discovery`: Data discovery scripts.
   - `lwh`: Logical data warehouse setup scripts.
   - `usp`: Stored procedures for gold layer aggregation.
   - `dwh`: Dedicated data warehouse scripts
3. **Run the Synapse Pipelines** to orchestrate the workflow.
4. **Connect to Power BI** for visualization and reporting.

