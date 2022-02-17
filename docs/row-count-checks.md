# Row count checks

## Row count threshold checks

Row count must be greater than 0:
```yaml
checks for CUSTOMERS:
  - count > 0
```

Row count must be between 1000 and 2000:
```yaml
checks for CUSTOMERS:
  - count between 1000 and 2000
```

See the [reference docs for all other threshold flavours](reference.md#numeric-check-thresholds)

## Filtered row count checks
There must be between 1000 and 2000 rows with category 'HIGH':
```yaml
checks for TABLE_NAME:
  - count between 1000 and 2000:
      filter: category = 'HIGH'
```

`filter` is a SQL condition that will be used as-is in the query, so include quoting appropriately.

This type of row count check is based on an [aggregation metric](reference.md#aggregation-queries):

## Cross table row count checks

Check if the row count of a table is the same as another table in the same data source
```yaml
checks for CUSTOMERS:
  - count same as RAW_CUSTOMERS
```

## Cross data source row count checks

Check if the row count of a table is the same as another table in another data source
```yaml
checks for CUSTOMERS:
  - count same as RAW_CUSTOMERS in other_snowflake_data_source
```

## Cross table row count checks with partitions

> (Coming soon)
>
> TODO Consider if we should push it to the user to define the right variables and avoid clashes between the variable names when comparing?
>
> Check if the row count of a table is the same as another table in the same data source
> ```yaml
> checks for CUSTOMERS [daily_date]:
>   - count same as RAW_CUSTOMERS [daily_timestamp]
> ```
> where in the same or another file:
> ```yaml
> partition for CUSTOMERS [daily_date]:
>   filter: date = DATE '${date}'
>
> partition for RAW_CUSTOMERS [daily_timestamp]:
>   filter: TIMESTAMP '${ts_start}' <= "ts" AND "ts" < TIMESTAMP '${ts_end}'
> ```
>
> Row count comparison with partition also works cross data source.
>
> Learn more on [Partitions](#partitions)

## Change over time row count checks

```yaml
checks for CUSTOMERS:
  - change for count < 50
  - change avg last 7 for count < 50
  - change min last 7 for count < 50
  - change max last 7 for count < 50
```