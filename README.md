# Novacart Case Study – End-to-End Lakehouse Pipeline in Databricks

Built an end-to-end data engineering pipeline in Databricks using Bronze-Silver-Gold architecture with incremental ingestion, control tables, quarantine handling, Gold current-state merges, SCD Type 2 history tracking, and category-level aggregations.

---

## 🚀 Project Overview

This project processes e-commerce transactional data (orders, products, payments) and transforms it into clean, validated, and business-ready datasets.

The pipeline is designed using a **Medallion Architecture**:
- **Bronze** → Raw incremental ingestion
- **Silver** → Data cleaning, validation, quarantine
- **Gold** → Business logic, aggregation, historical tracking

---

## 🏗️ Architecture

![Architecture](architecture/novacart_architecture.png)



### Data Flow:
Source → Bronze → Silver → Gold → Analytics

---

## ⚙️ Tech Stack

- Databricks
- PySpark
- Delta Lake
- Unity Catalog
- SQL

---

## 🥉 Bronze Layer (Raw Ingestion)

- Incremental ingestion from source (Azure SQL / external catalog)
- Watermark logic using:
  - Timestamp (`updated_at`, `processed_at`)
  - Primary key (tie-breaker)
- Metadata columns added:
  - `bronze_ingested_at`
  - `bronze_run_id`
- Control table tracks ingestion state

### 🔎 Bronze Control Table

![Bronze Control](screenshots/bronze_control_table.png)

---

## 🥈 Silver Layer (Cleaning & Validation)

- Standardization:
  - product_name cleaning (remove `_`, `-`)
  - category normalization
  - numeric conversions (price, amounts)
- Data Quality Rules:
  - null checks
  - invalid price handling
  - invalid categories filtered
- Quarantine handling:
  - bad records stored separately

### 🔎 Silver Processing Control

![Silver Control](screenshots/silver_processing_control.png)

### ⚠️ Quarantine Example

![Quarantine](screenshots/quarantine_examples.png)

---

## 🥇 Gold Layer (Business Layer)

### Features:
- Incremental processing using impacted records
- Multi-table joins:
  - orders + products + payments
- Business metrics:
  - payment ratios
  - payment states
- Category-level aggregation

### 🔎 Orders Information Table

![Gold Orders](screenshots/gold_orders_information.png)

---

## 📊 Category Performance

Aggregated business metrics:
- Total orders
- Gross Merchandise Value (GMV)
- Total amount paid
- Payment completion ratio
- Payment failure rate

![Category Performance](screenshots/category_performance.png)

---

## 🧠 SCD Type 2 Implementation

Maintains historical changes:
- `valid_from_ts`
- `valid_to_ts`
- `is_current`

Tracks changes in:
- order status
- product details
- payment updates

---

## 🔄 Incremental Processing Design

- Bronze → watermark-based ingestion
- Silver → incremental read using control table
- Gold → impacted-record-based processing
- Idempotent pipeline (safe re-runs)

---

## 📁 Project Structure
