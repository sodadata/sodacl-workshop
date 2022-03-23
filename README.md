# SodaCL Demo 

Use this demo environment to see Soda in action! Configure SodaCL checks in the [checks.yml](/sodacl/checks.yml) file, then run scans using Soda Core to witness the command-line output of your checks for data quality.

**Soda Checks Language (Beta)** is a domain-specific language for data reliability. You use SodaCL to define Soda Checks in a checks YAML file. A Soda Check is a test that **Soda Core (Beta)**, Soda’s open-source, command-line tool, executes when it scans a dataset in your data source. 



## Prerequisites
- Window or MacOS Terminal
- Docker downloaded and installed in your local environment
- a code editor 

## Set up

1. Clone this repo locally, then use Terminal to navigate to the repo in your local environment.
2. If it is not already running in your local environment, start Docker.
3. In Terminal, run the following command to set up the prepared demo environment:
`docker-compose up`
4. When the `database system is ready to accept connections`, open a new Terminal tab or window and run the following command to open a shell with the Soda files.
`docker-compose exec soda-core /bin/bash`
The command results in a prompt similar to the following:
`root@80e6a2167613:/sodacl#`
5. To test that the environment is working properly, run a Soda scan: 
`soda scan -d adventureworks -c configuration.yml checks.yml` 
The command output results in the following output:
```shell
Soda Core 0.0.1
Scan summary:
1/1 check PASSED:
    dim_account in adventureworks
      row_count between 20 and 100 [PASSED]
All is good. No failures. No warnings. No errors.
```

## About the demo data
The demo PostgreSQL database contains data from a fictitious, multinational manufacturing company called Adventure Works Cycles. It contains tables for customer information, employee information, sales territories, sales information, product categories, and more. 

To browse the contents of the database,  use the database connection details in the [configuration.yml](/sodacl/configuration.yml) file to connect to the database from your code editor, or refer to the [schema diagram](adventureworks.png).


## Get started

1. Review [SodaCL: The Basics](#sodacl-the-basics) if you wish.
2. Open the [checks.yml](/sodacl/checks.yml) file in your code editor or Terminal. This file is where you define Soda Checks using SodaCL. You can edit the existing file to adjust the existing check and add others, or you can create a new YAML file to write your own checks. 
3. Reference the documentation for a list of [built-in SodaCL checks](https://docs.soda.io/soda-cl/soda-cl-overview.html) to write your own checks that look for bad-quality data in the demo database.
4. When you have written a check or two, use the command-line to run another soda scan. If you created your own YAML file to write checks, replace `checks.yml` with your own file name in the soda scan command: 
`soda scan -d adventureworks -c configuration.yml checks.yml`
5. Write more checks and run more scans! Explore the different built-in metrics and how they work in the context of a check. 

To exit the shell, use the command `exit`.
To stop the Docker container, use `Ctrl+c.`
To learn more about Soda Core (Beta), access the [Soda Core documentation](https://docs.soda.io/soda-core/overview.html).

## SodaCL: The Basics

**Soda Core** is a free, open-source, command-line tool that enables you to use the **Soda Check Language (SodaCL**) to turn user-defined input into aggregated SQL queries. When it runs a scan on a dataset, Soda Core executes the checks to find invalid, missing, or unexpected data. When your Soda Checks fail, they surface the data that you defined as "bad". 

In the real world, you would connect Soda Core to your data source (Snowflake, BigQuery, etc.), then define your Soda Checks for data quality in a YAML file, and use Soda Core to regularly run scans of your data to execute the checks. In the context of this demo environment, we’ve simplified the experience to focus not on Soda Core as a tool, but on SodaCL, the language you use to define Soda Checks.

A **Soda Check** is a test that Soda Core performs when it scans a dataset in your data source. Technically, a check is a Python expression that, during a scan, checks metrics to see if they match the parameters you defined for a measurement.

During a scan, checks return a status of pass, fail, warn, or error. 
- If a check **passes**, you know your data is sound. 
- If a check **fails**, it means the scan discovered data that falls outside the expected or acceptable parameters you defined in your check.
- If a check triggers a **warning**, it means the data falls within the parameters you defined as “worthy of a warning” in your check.
- If a check returns an **error**, it means there is a problem with the check itself, such as a syntax error.

The following Soda Check uses SodaCL to make sure that the `dim_account` dataset contains rows or, in other words, is not empty. In this example, `row_count` is the metric, and `0` is the measurement.
```yaml
checks for dim_account:
  - row_count > 0
```
<br />

You write Soda Checks using SodaCL’s built-in metrics. The example above is one of the simplest checks you can run on a single dataset, but SodaCL offers a wealth of built-in metrics that enable you to define checks for more complex situations. Reference the full [SodaCL (Beta) documentation](https://docs.soda.io/soda-cl/soda-cl-overview.html) for details. A few examples follow.


**Check if duplicate values exist in a column in a dataset**
```yaml
checks for dim_customer:
 - duplicate_count(last_name) = 0
```

**Issue a warning if an expected column in a dataset is missing**
```yaml
checks for dim_customer:
  - schema:
      warn:
        when required column missing: [last_name, email_address, sombrero]
```

**Check for valid values in a column in a dataset**
```yaml
checks for dim_product:
  - invalid_percent(color) < 1%:
      valid values:
        - red
        - yellow
        - green
```


SodaCL seeks to include non-developers in the search for bad data. You define Soda Checks in a **checks.yml** file in relatively plain language – relative to SQL queries, that is – then simply save the file and run a scan to execute the checks.

For this program, the [checks.yml](/sodacl/checks.yml) file in the `sodacl` directory is where you define Soda Checks using SodaCL. You can edit the existing file to adjust the existing check and add others, or you can create a new YAML file to write your checks and run Soda scans, replacing `checks.yml` with your own file name in the soda scan command.

`soda scan -d adventureworks -c configuration.yml checks.yml`


## Need help?

Let us know if you get stuck or have a question. 

Join the [Soda Community](https://community.soda.io/slack) in Slack, and find us in the [#soda-core](https://soda-community.slack.com/archives/C038FFU79J5) channel.