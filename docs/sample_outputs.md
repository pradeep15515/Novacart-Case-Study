# Sample Outputs

## Bronze Layer

### Ingestion Control Table

Tracks:
- last successful timestamp
- last processed primary key
- rows written per run

![Bronze Control](../screenshots/bronze_control_table.png)

---

## Silver Layer

### Cleaned Data

- Standardized product names
- Cleaned price fields
- Valid categories enforced

---

### Quarantine Table

Invalid records stored separately:

Examples:
- null product_name
- invalid price
- unknown category

![Quarantine Example](../screenshots/quarantine_examples.png)

---

## Gold Layer

### Orders Information Table

- Joined dataset from orders, products, payments
- Contains business-ready fields

![Gold Orders](../screenshots/gold_orders_information.png)

---

### SCD Type 2 Table

Tracks historical changes:
- valid_from_ts
- valid_to_ts
- is_current flag

---

### Category Performance Table

Aggregated metrics:
- total_orders
- gross_merchandise_value
- total_amount_paid
- avg_payment_completion_ratio
- payment_failure_rate

![Category Performance](../screenshots/category_performance.png)

---

## Sample Queries

### Category Summary

```sql
SELECT category, COUNT(*)
FROM novacart_casestudy.gold_schema.category_performance
GROUP BY category;
