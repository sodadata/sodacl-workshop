# Partitions

Large datasets often are built up incrementally.  Each job execution appends the new data to a table.
For those situations, Soda SQL has partitions.

Creating a partition is done on a table by specifying a filter SQL expression.
Like in this example a `daily` partition is defined on the `CUSTOMERS` table:
```yaml
partition for CUSTOMERS [daily]:
  filter: TIMESTAMP '${ts_start}' <= "ts" AND "ts" < TIMESTAMP '${ts_end}'
```

In the partition filter expression, reference is made to variables `ts_start` and `ts_end`.
These variables will have to be passed into the scan configuration.

Next you can put checks on a given partition like this:
```yaml
checks for CUSTOMERS [daily]:
  - count = 6
  - missing(cat) = 2
```

> [Note] that in a single scan configuration file, you can declare both checks on the full table as well as on a partition.
