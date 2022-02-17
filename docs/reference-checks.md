# Reference checks

Basic reference check verifies that a value is present in another column.

```yaml
checks for ORDERS:
  - reference from (customer_id) to CUSTOMERS (id)
```

Multi-column reference check
```yaml
checks for ORDERS:
  - reference from (customer_country, customer_zip) to CUSTOMERS (country, zip)
```

> [Note] Missing values in the source columns are also considered invalid.