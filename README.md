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

![Bronze Control]
<img width="3020" height="1644" alt="image" src="https://github.com/user-attachments/assets/b25f0985-260b-4f29-9323-83029f0a4abc" />


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

![Silver Control]
<img width="2992" height="1452" alt="image" src="https://github.com/user-attachments/assets/2848624e-ffbb-4cbb-92a8-04bdbf98358e" />


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

![Category Performance]
<img width="2984" height="1624" alt="image" src="https://github.com/user-attachments/assets/c8df4f3d-83fb-469c-b33e-8d0d53368a9f" />


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
## 📊 Additional Implementations

### 📈 Dashboard (Analytics Layer)

- Built dashboard on top of Gold tables
- Visualized:
  - Category performance (revenue, orders)
  - Payment completion ratios
  - Payment failure rates
- Enabled business-level insights from processed data

![Dashboard]
<img width="1500" height="734" alt="Screenshot 2026-04-03 at 2 59 45 AM" src="https://github.com/user-attachments/assets/afc0c4bb-b345-405f-946d-603f74d35ebd" />
<img width="1483" height="812" alt="Screenshot 2026-04-03 at 3 00 13 AM" src="https://github.com/user-attachments/assets/6a5d99a5-c786-4a87-8eca-51cd4a06ba19" />


### ⚙️ Workflow Orchestration

- Created Databricks Workflows to automate pipeline execution
- Orchestrated:
  - Bronze ingestion → Silver processing → Gold processing
- Ensured dependency-based execution across layers

![Workflow](screenshots/workflow.png)
<img width="1502" height="827" alt="Screenshot 2026-04-03 at 3 01 51 AM" src="https://github.com/user-attachments/assets/8ecd6300-a355-41a1-84bb-8aebc0efdfab" />

<img width="1498" height="806" alt="Screenshot 2026-04-03 at 3 02 20 AM" src="https://github.com/user-attachments/assets/75feb074-c96b-4b59-bf98-1c56cb5942c2" />
<img width="1498" height="824" alt="Screenshot 2026-04-03 at 3 03 11 AM" src="https://github.com/user-attachments/assets/a9967011-80c7-460a-af91-a371d7a2178a" />

--
### 🚨 Alerts & Monitoring

- Configured alerts for:
  - Failed jobs
  - Data anomalies
  - Pipeline execution failures
- Enabled monitoring of pipeline health

![Alerts]

<img width="1157" height="631" alt="Screenshot 2026-04-03 at 3 04 02 AM" src="https://github.com/user-attachments/assets/2e9a006e-ecf4-49e3-a1e5-91e87147b6ab" />
<img width="2234" height="1316" alt="image" src="https://github.com/user-attachments/assets/59790ca0-83b0-4d1b-aeec-45df94526005" />







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
