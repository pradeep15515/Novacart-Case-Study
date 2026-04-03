# Project Overview

## Introduction

This project implements an end-to-end Lakehouse data pipeline using Databricks for an e-commerce dataset (Novacart). The pipeline processes raw transactional data (orders, products, payments) and transforms it into clean, validated, and business-ready datasets.

The architecture follows the Medallion pattern (Bronze, Silver, Gold) using Delta Lake.

---

## Architecture Summary

The pipeline consists of three main layers:

### Bronze Layer
- Ingests data incrementally from source system (Azure SQL / external catalog)
- Uses timestamp + primary key watermark logic
- Stores raw data with ingestion metadata
- Maintains ingestion control table

### Silver Layer
- Cleans and standardizes data
- Applies data validation rules
- Separates valid and invalid data (quarantine)
- Uses processing control table for incremental logic

### Gold Layer
- Builds business-ready datasets
- Implements incremental updates based on impacted records
- Maintains:
  - Current state table
  - SCD Type 2 history table
  - Aggregated category performance metrics

---

## Key Components

- Incremental ingestion using watermark (timestamp + primary key)
- Delta Lake MERGE operations for upserts
- Control tables for tracking pipeline state
- Data quality validation and quarantine handling
- SCD Type 2 implementation for historical tracking
- Category-level business aggregation

---

## Key Features

- Idempotent pipeline design
- Incremental processing across all layers
- Quarantine handling for bad data
- Late-arriving data handling using lookback window
- Business KPI calculations (revenue, payment ratios, failure rates)
- Snapshot exports for historical analysis

---

## Technology Stack

- Databricks
- PySpark
- Delta Lake
- Unity Catalog
- SQL

---

## Data Flow

Source → Bronze → Silver → Gold → Analytics

- Bronze: Raw ingestion
- Silver: Cleaned + validated data
- Gold: Business-ready datasets + aggregations
