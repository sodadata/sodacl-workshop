# SodaCL Preview Program

Soda Checks Language (SodaCL) is a new, declarative language designed to empower a broad range of users to write data reliability checks using a flexible, accessible syntax. With a rich library of built-in metrics, nearly anyone can use SodaCL to find invalid, missing, or unexpected data.

Refer to the [SodaCL Preview Program Instructions](/instructions.md) for further details.

## Set up
1. In Terminal, run the following command to set up the prepared testing environment:
`docker-compose up`
2. When the `database system is ready to accept connections`, open a new Terminal tab or window and run the following command to open a shell with the SodaCL files.
`docker-compose exec soda-core /bin/bash`
The command results in a prompt similar to the following:
`root@80e6a2167613:/sodacl#`
3. To test that the environment is working properly, run a Soda scan: 
`soda scan -d adventureworks -c configuration.yml -ch checks.yml` 
The command output results in the following output:
```shell
Soda Core 0.0.1
Scan summary:
1/1 check PASSED:
    dim_account in adventureworks
      count between 20 and 100 [PASSED]
All is good. No failures. No warnings. No errors.
```

## Add or edit Soda Checks

The [checks.yml](/sodacl/checks.yml) file in the `sodacl` directory is where you define Soda Checks using SodaCL. You can edit the existing file to adjust the existing check and add others, or you can create a new YAML file to write your checks and run Soda scans, replacing `checks.yml` with your own file name in the soda scan command: 

`soda scan -d adventureworks -c configuration.yml -ch checks.yml`

Refer to the [SodaCL Preview Program Instructions](/instructions.md) for further details.

## About the demo data
The demo PostgreSQL database contains data from a fictitious, multinational manufacturing company called Adventure Works Cycles. It contains a dimensional model schema with dimension and facts tables which house information such as customer details, employee into, sales territories, sales information, product categories, and more. 

To browse the contents of the database, use the DB connection details in the [configuration.yml](/sodacl/configuration.yml) file to connect to the database from your code editor, or refer to the schema diagram below.

#### Schema diagram
![adventureworks](adventureworks.png)

  
