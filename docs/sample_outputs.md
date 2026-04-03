# Sample Outputs

## Bronze Layer

### Ingestion Control Table

Tracks:
- last successful timestamp
- last processed primary key
- rows written per run


<img width="2086" height="382" alt="image" src="https://github.com/user-attachments/assets/29093e8f-68a5-4600-a248-bb1165570f9d" />
<img width="2096" height="358" alt="image" src="https://github.com/user-attachments/assets/c1feaaf2-a551-4a29-9be5-630b48e4846f" />



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

![Quarantine Example]
<img width="2104" height="1072" alt="image" src="https://github.com/user-attachments/assets/319f9a2a-c213-43e4-9d48-2d1fc186bd22" />


---

## Gold Layer

### Orders Information Table

- Joined dataset from orders, products, payments
- Contains business-ready fields

![Gold Orders]
<img width="2092" height="960" alt="image" src="https://github.com/user-attachments/assets/eb968ff0-239d-468b-ab61-ce36b796ca77" />
<img width="2092" height="972" alt="image" src="https://github.com/user-attachments/assets/de984f3b-a290-477b-ae7f-f91818448228" />


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

![Category Performance]
<img width="2104" height="754" alt="image" src="https://github.com/user-attachments/assets/9338b10e-5393-4b26-bc32-2475c1430d69" />


---

## Sample Queries

### Category Summary

```sql
SELECT category, COUNT(*)
FROM novacart_casestudy.gold_schema.category_performance
GROUP BY category;
