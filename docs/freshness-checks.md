# Freshness checks

A freshness check measures how old the youngest row in a table is. It is to ensure that the latest data is
available.  Data is considered fresh when the time between the oldest row in a table and "now" is
not too big.  Typically tables have a `created_at` timestamp or so to indicate when the object was created.
So if you see rows with a `created_at` of not that long ago, then you know that you have fresh data.

Take into account that these freshness checks are not evaluated constantly.  They are executed on the
scan schedule.  So if you want to be alerted if data is not available and fresh by 9am, you need a
schedule that executes a scan at 9am daily and have checks for availability (row count) and freshness.

```yaml
checks for CUSTOMERS:
  - freshness using row_added_ts < 1h
```

"now" by default will be taken from the scan variable `data_end_timestamp` if it is specified.  If this variable
is not configured, check evaluation time will be used.  It is recommended to use a variable so that checks
can be re-executed after for example [backfilling](https://www.startdataengineering.com/post/how-to-backfill-sql-query-using-apache-airflow/).

If you pass a variable with a timestamp, it should use the iso 8601 format.

The next example shows how to specify a specific variable name. To use variable `now_timestamp` as the value for now:

```yaml
checks for CUSTOMERS:
  - freshness using row_added_ts with now_timestamp < 1h
```

Syntax for freshness check: `freshness using' {column_name} [with {now_variable_name}] < {duration}` where
`[...]` indicates that the now variable name is optional
* `column_name` is the name of a timestamp column in the table
* `now_variable_name` is optional and specifies the timestamp variable name to use for "now"
* `duration` is an indication of the max time in one of these formats
  * `3d` 1 days
  * `1d6` 1 day and 6 hours
  * `1h` 1 hour
  * `1h30` 1 hour and 30 minutes
  * `30m` 30 minutes
  * `3m30` 3 minutes and 30 seconds

To specify a warn level or both a fail and a warn level.  The next example shows how to generate a
warning if the most recent data is more than 1 hour old, and a failure if the data is more than 12 hours old.
```yaml
checks for CUSTOMERS:
  - freshness using row_added_ts:
      warn: when > 1h
      fail: when > 12h
```

### Timezone handling
- Freshness check assumes UTC timezone if no timezone information is provided either in the check timestamp or in the checked data coming from the data source.
- Both timestamps are converted to UTC so that comparison works as expected.
