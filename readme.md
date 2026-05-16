# E-Commerce Medallion Data Engineering Project

## Project Overview
This project demonstrates an enterprise-grade **Medallion Architecture (Bronze → Silver → Gold)** using **Microsoft Fabric, ADLS Gen2, Data Pipelines, Lakehouse, and PySpark**.

The solution transforms raw e-commerce CSV files from Azure Data Lake Storage Gen2 into analytics-ready Gold models for customer intelligence, operational reporting, and business dashboards.

---

# Architecture Flow

ADLS Gen2 (Raw CSV Files)  
↓  
Microsoft Fabric Pipeline (Get Metadata + ForEach + Copy Activity)  
↓  
Bronze Lakehouse (Raw Parquet Storage)  
↓  
Silver Lakehouse (Data Cleaning, Standardization, Duplicate Handling)  
↓  
Gold Lakehouse (Customer 360 Analytics Model)  
↓  
Power BI / Reporting

---

# Bronze Layer (Raw Ingestion)

## Source:
- Azure Data Lake Storage Gen2
- Folder-based source ingestion (`sourcedata`)

## Pipeline Features:
- Get Metadata activity for dynamic file discovery
- ForEach loop for scalable ingestion
- Copy Data activity
- CSV → Parquet transformation
- Snappy compression
- Landing into Bronze Lakehouse

## Bronze Tables:
- customers
- orders
- payments
- support_tickets
- web_activities

## Purpose:
- Preserve raw source data
- Centralize ingestion
- Maintain source-of-truth
- Support replayability and auditing

---

# Silver Layer (Data Cleaning & Standardization)

## Key Transformations:
- Mixed date standardization:
  - `20220722`
  - `22/07/2022`
  - `2022-07-22`
  → Standardized to `yyyy-MM-dd`

- Null / invalid replacements:
  - `n/a` → Default date
  - Null statuses → `Unknown`
  - `NA` → `No Issue`

- Data quality:
  - Email normalization
  - Gender standardization
  - Payment method formatting
  - Resolution status cleanup
  - Page viewed cleanup

- Duplicate tracking:
  - `row_number()` over business keys

---

## Silver Tables:
- cst_silver
- orders_silver
- payments_silver
- support_tickets_silver
- web_activities_silver

---

# Gold Layer (Business-Ready Models)

## Main Gold Model:
### `customers_360`

## Combines:
- Customer profile
- Orders history
- Payment activity
- Support tickets
- Web activity

## Use Cases:
- Customer Lifetime Value
- Customer Behavior Analytics
- Support Trend Analysis
- Payment Performance
- Web Engagement Insights

---

# Tech Stack
- Microsoft Fabric
- Azure Data Lake Storage Gen2
- Fabric Data Pipeline
- Lakehouse
- PySpark
- Delta / Parquet
- Power BI
- GitHub

---

# Project Structure

```bash
ADLS/
└── Raw CSV Files

Pipeline/
└── ADLS_to_Bronze_Pipeline.json

Bronze/
└── Raw Parquet Tables

Silver/
├── cst_silver
├── orders_silver
├── payments_silver
├── support_tickets_silver
└── web_activities_silver

Gold/
└── customers_360