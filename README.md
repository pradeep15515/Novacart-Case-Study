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
<img width="940" height="764" alt="databricks_architecture" src="https://github.com/user-attachments/assets/34a9862a-e85d-4e08-a478-fbbd2425d229" />



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
novacart-case-study/
│
├── notebooks/
│   ├── 01_bronze_ingestion.py
│   ├── 02_silver_processing.py
│   └── 03_gold_processing.py
│
├── docs/
│   ├── project_overview.md
│   ├── assumptions.md
│   └── sample_outputs.md
│
├── screenshots/
│   ├── bronze_control_table.png
│   ├── silver_processing_control.png
│   ├── gold_orders_information.png
│   ├── category_performance.png
│   └── quarantine_examples.png
│
└── README.md

---

## ▶️ How to Run

1. Create catalog and schemas in Databricks
2. Run Bronze ingestion notebook
3. Run Silver processing notebook
4. Run Gold processing notebook
5. Validate using SQL queries

---

## ⚠️ Assumptions

- Source data uses reliable timestamps
- No hard deletes in source system
- Categories limited to:
  - ELECTRONICS
  - FITNESS
  - LIFESTYLE
- Invalid records routed to quarantine

---

## 🚧 Limitations

- No real CDC (timestamp-based only)
- No orchestration (manual execution)
- No streaming pipeline
- No monitoring/alerting framework

---

## 🔮 Future Improvements

- Add Databricks Workflows / Airflow orchestration
- Implement CDC ingestion
- Add data quality framework (Great Expectations)
- Add streaming (Kafka / Event Hub)
- Schema evolution handling

---

## 💡 Key Takeaways

- Built a production-style data pipeline using Delta Lake
- Implemented incremental ingestion and control tables
- Designed data quality and quarantine strategy
- Developed SCD Type 2 history tracking
- Delivered business-ready aggregated datasets

---
