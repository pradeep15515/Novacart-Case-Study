# Assumptions

## Data Assumptions

- `order_id`, `product_id`, and `payment_id` are unique identifiers
- Source timestamps (`updated_at`, `processed_at`) are reliable for incremental processing
- Source system does not perform hard deletes
- Data may contain inconsistencies such as:
  - invalid categories
  - null values
  - malformed price fields

---

## Pipeline Assumptions

- Bronze layer is append-only
- Incremental ingestion is based on:
  - timestamp column
  - primary key for tie-breaking
- Silver layer keeps the latest record per business key
- Gold layer recomputes only impacted records (not full reload)
- Control tables track last successful runs

---

## Data Quality Rules

### Products
- product_name must not be null
- price must be > 0
- category must be valid

### Orders
- customer_id must exist
- product_id must exist
- order_amount must be > 0

### Payments
- payment_status must not be null
- paid_amount must be > 0

Invalid records are written to quarantine tables.

---

## Category Standardization

Only the following categories are considered valid:
- ELECTRONICS
- FITNESS
- LIFESTYLE

Invalid categories (e.g., ELECTRNICS) are:
- corrected where possible
- otherwise sent to quarantine

---

## Limitations

- No real CDC (Change Data Capture), only timestamp-based incremental logic
- No orchestration tool (manual execution)
- No schema evolution handling
- No streaming pipeline (batch only)

---

## Future Improvements

- Implement orchestration (Databricks Workflows / Airflow)
- Add schema evolution support
- Introduce CDC-based ingestion
- Build streaming pipeline for real-time processing
