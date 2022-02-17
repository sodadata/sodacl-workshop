# For each checks

## For each table

For each table allow to specify a list of checks on a group of tables.  Several styles of
referring to tables can be combined:
```yaml
for each table T:
  tables:
    # Include the table name, in the SodaCL file's default data source
    - TEST_CUSTOMERS
    # Include all table names matching this wildcard expression, in the SodaCL file's default data source
    - PROD_%
    # Include all table names matching this wildcard expression, in the specified data source
    - snwflk1_prd.%_CUSTOMERS
    # The include directive is optional, it can be used to make the list more readable in case excludes are also specified
    - include TEST_CUSTOMERS
    # Exclude a specific table name, in the SodaCL file's default data source
    - exclude SALARIES
    # Exclude a tables matching this wildcard, in the SodaCL file's default data source
    - exclude SALAR%
    # Exclude a table in the specified data source
    - exclude other_data_source.SALAR%
  checks:
    - count > 0
    - missing(id) = 0
```

The purpose of the table name `T` is only to ensure that every `for each` check has a unique name.

Both data source name and table name filters can use `%` as a wildcard.

A data source name can optionally be specified by using a dot `.` between the data source name filter
and the table name filter.

Matching is done case insensitive.

## For each column

> (Coming soon)
>
> Columnsets allow to specify a list of checks on group of columns:
> ```yaml
> columnset customers_columnset:
>   - include %_CUSTOMERS.name_%
>   - include snwflk1_tst.TEST_CUSTOMERS.name_%
>   - in {tablesetname} include
>      - colone
>      - coltwo
>      - col%
>
> checks for each column c in customers_columnset:
>   - missing(c) = 0
>   - avg(c) between 50 and 100
> ```
>
> Idea: columnsets filtered on cloud properties:
> ```yaml
> columnset pii_columnset:
>   data domain: Sales
>   soda cloud  filters:
>     tag: pii
>     owner: theotherone@ourcompany.com
>
> `checks for each column c in pii_columnset:
>   - missing(c) = 0
>   - avg(c) between 50 and 100
> ```

## For each group

> (Coming soon)
