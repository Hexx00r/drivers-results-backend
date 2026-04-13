# drivers-results-backend
# Formula 1 Data Engineering Pipeline
A production-grade Medallion Architecture built on **Azure Databricks** and **Unity Catalog**.

## 🏗️ Architecture
This project implements a three-tier lakehouse architecture to process historical F1 data.

- **Bronze**: Raw JSON ingestion from Azure Data Lake (ADLS Gen2).
- **Silver**: Data cleaning, schema enforcement (PySpark), and \N marker handling.
- **Gold**: Business-level aggregations and driver career statistics.

## 🛠️ Tech Stack
- **Languages**: Python (PySpark), SQL
- **Platform**: Azure Databricks (Runtime 15.4 LTS)
- **Governance**: Unity Catalog
- **Storage**: Azure Data Lake Storage Gen2 (Delta Lake format)
- **CI/CD**: Databricks Repos + GitHub

## 📊 Data Quality Checks
The pipeline includes automated DQ gates to monitor:
- Null counts for critical columns (DOB, Position).
- Schema validation.
- Ingestion timestamps for lineage.

## 🚀 How to Run
1. Configure Azure Storage Credentials in Unity Catalog.
2. Run `01_ingest_bronze` to land raw data.
3. Run `02_transform_silver` for cleaning.
4. Run `03_presentation_gold` for the final analytics table.


# 🏎️ F1 Data Engineering Pipeline (Medallion Architecture)

This repository contains a production-ready data pipeline built on **Azure Databricks** using **Unity Catalog** and **Delta Lake**.

## 📐 Architecture Diagram
```mermaid
graph TD
    subgraph "Azure Data Lake Storage (Bronze)"
        A[Raw JSON Drivers/Results]
    end

    subgraph "Databricks Silver Layer"
        A --> B{Transform & Clean}
        B --> C[(drivers_silver)]
        B --> D[(results_silver)]
    end

    subgraph "Databricks Gold Layer"
        C & D --> E[Join & Aggregate]
        E --> F[(driver_career_stats)]
    end

    subgraph "Presentation"
        F --> G[Power BI / SQL Warehouse]
    end