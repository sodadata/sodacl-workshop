# SodaCL Preview Program Instructions

Thank you for joining the Soda Checks Language (SodaCL) Preview Program. 

We hope to ask for your honest feedback as you complete the short exercises that ask you to prepare a few Soda Checks for data quality in a dataset, then run a scan of the data to execute the checks and find out which data is good and which is bad.


## Prerequisites
- Window or MacOS Terminal
- Docker downloaded and installed on your local environment
- a code editor 

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

## About the demo data
The demo PostgreSQL database contains data from a fictitious, multinational manufacturing company called Adventure Works Cycles. It contains tables for customer information, employee information, sales territories, sales information, product categories, and more. 

To browse the contents of the database, use the DB connection details in the [configuration.yml](/sodacl/configuration.yml) file to connect to the database from your code editor, or refer to the [schema diagram](adventureworks.png).


## Get started

1. Review [SodaCL: The Basics](#sodacl-the-basics) if you wish.
2. Open the checks.yml file in your code editor or Terminal. The [checks.yml](/sodacl/checks.yml) file is where you define Soda Checks using SodaCL. You can edit the existing file to adjust the existing check and add others, or you can create a new YAML file to write your checks. 
3. Reference the included list of built-in SodaCL metrics and check examples to write your own checks that look for bad-quality data in the demo database.
4. When you have written a check or two, use the command-line to run another soda scan. If you created your own YAML file to write checks, replace `checks.yml` with your own file name in the soda scan command: 
`soda scan -d adventureworks -c configuration.yml -ch checks.yml`
5. Write more checks and run more scans! Explore the different built-in metrics and how they work in the context of a check. 
6. Make notes as you go so you can share your experiences with SodaCL with our program coordinators.

To exit the shell, use the command `exit`.
To stop the Docker container, use ctrl+c.

## SodaCL: The Basics

**Soda Core** is a free, open-source, command-line tool that enables you to use the **Soda Check Language (SodaCL**) to turn user-defined input into aggregated SQL queries. When it runs a scan on a dataset, Soda Core executes the checks to find invalid, missing, or unexpected data. When your Soda Checks fail, they surface the data that you defined as "bad". 

In the real world, you would connect Soda Core to your data source (Snowflake, Postgres, etc.), then define your Soda Checks for data quality in a YAML file, and use Soda Core to regularly run scans of your data to execute the checks. In the context of this program, we’ve simplified the experience to focus not on Soda Core as a tool, but on SodaCL, the language you use to define Soda Checks.

A **Soda Check** is a test that Soda Core performs when it scans a dataset in your data source. Technically, a check is a Python expression that, during a scan, checks metrics to see if they match the parameters you defined for a measurement.

During a scan, all checks return a status of pass, fail, warn, or error. 
- If a check **passes**, you know your data is sound. 
- If a check **fails**, it means the scan discovered data that falls outside the expected or acceptable parameters you defined in your check.
- If a check **warns**, it means the data falls within the parameters you defined as “worthy of a warning” in your check.
- If a check returns an **error**, it means there is a problem with the check itself, such as a syntax error.

The following Soda Check uses SodaCL to make sure that the `CUSTOMERS` dataset contains rows or, in other words, is not empty. In this example, `count` is the metric, and `0` is the measurement.
```yaml
checks for CUSTOMERS:
  - count > 0
```

You write Soda Checks using SodaCL’s built-in metrics. (Actually, you can go beyond the built-in metrics and write your own SQL queries, but that is an exercise for another day.) The example above is one of the simplest checks you can run on a single dataset, but SodaCL offers a wealth of built-in metrics that enable you to define checks for more complex situations.

**Check that a value is present in another column**
```yaml
checks for ORDERS:
  - reference from (customer_id) to CUSTOMERS (id)
```

**Issue a warning if an expected column in a dataset is missing**
```yaml
checks for CUSTOMERS:
  - schema:
      warn:
        when required column missing: [id, sizetxt, distance]
```

**Check for valid values in a column in a dataset**
```yaml
checks for CUSTOMERS:
  - invalid_percent(category) < 1%:
      valid values:
        - HIGH
        - MEDIUM
        - LOW
```


As you may have noticed, the SodaCL language seeks to include non-developers in the search for bad data. You define Soda Checks in a **checks.yml** file in relatively plain language – relative to SQL queries, that is – then simply save the file and run a scan to execute the checks.

For this program, the [checks.yml](/sodacl/checks.yml) file in the `sodacl` directory is where you define Soda Checks using SodaCL. You can edit the existing file to adjust the existing check and add others, or you can create a new YAML file to write your checks and run Soda scans, replacing `checks.yml` with your own file name in the soda scan command: 

`soda scan -d adventureworks -c configuration.yml -ch checks.yml`


## Need help?

Let us know if you get stuck or have a question. 

Join the [Soda Community](https://community.soda.io/slack) in Slack, and find us in the [#sodacl-preview-program](https://soda-community.slack.com/archives/C032BDZN3HV) channel.


## Soda Testing Terms & Conditions

The present terms and conditions apply to the contractual relationship (“Agreement”) between Soda Data NV, having its registered office and place of business at Rue Picard 7, 1000, Brussels, Belgium ("Soda") and yourself (“Participant”).

1.Object. Soda will make available to Participant, during the period of February 14, 2022, to March 4, 2022 ("Program Period") and for testing and evaluation purposes only, the pre-release version of the Soda Checks Language (SodaCL) product and related documentation (the "Product") under the conditions specified by Soda.

2.License. Soda grants to Participant a free, limited, non-exclusive, revocable, non-transferable license, during the Program Period, to use the Product strictly for the purpose of internal testing and evaluation of the Product and providing feedback to Soda. Participant shall not use the Product for the processing of any data other than the data provided by Soda. Participant shall not attempt to modify, create derivative works of, decompile, disassemble, reverse engineer or obtain access to the source code of the Product and shall not disclose, sell, distribute, sublicense, transfer, print, or copy, in whole or in part, the Product. In no event shall Participant use the Product for any commercial purpose.

3.Feedback. During the Program Period, Participant will participate in information sessions and calls relating to the Product if requested by Soda, test and evaluate the Product, and provide feedback to Soda (verbally or by means of a written report, as requested by Soda) concerning the functionality and performance of the Product as reasonably requested by Soda, including, without limitation, identifying potential errors, imperfections, improvements, or modifications ("Feedback"). Participant irrevocably assigns to Soda, free of charge, any and all rights in the Feedback immediately upon communication of such Feedback, and Soda may use this Feedback for any purpose without compensation or attribution to Participant. Participant also grants Soda the right to include, free of charge, Participant's name and role together with Participant's Feedback as reference in promotional materials, including Soda's website.


4.Compensation. In consideration for Participant's participation under this Agreement, Soda shall provide a limited edition swag kit to Participant with a total value of fifty (50) EUR / fifty (50) USD and Soda will provide a Amazon gift voucher to Participant with a value of three hundred (300) EUR / three hundred (300) USD. Participant accepts exclusive liability for the payment of all taxes and contributions which are measured by the fees paid to Participant. Soda will also grant Participant a one-off 3-month full, free access to Soda Cloud with extended support from Soda in case Participant posts a writeup relating to the Product into Participant's community and network. Any writeup must be previously fact-checked by Soda and comply with applicable legislation.

5.Title. This Agreement does not give Participant any title or interest in the Product. The Product and any derivative works thereof and all copyright, patent, and other proprietary rights therein or associated therewith are and shall remain the sole property of Soda, even if Soda incorporates any Feedback into the Product.

7.Confidentiality. Confidential information means any non-public information of any form obtained by Participant in the performance of this Agreement, including, but not limited to, information concerning the Product, performance data, test results, business, financial and product development plans and strategies. Participant also agrees to treat any and all Feedback as confidential information. Participant agrees to hold confidential information in strict confidence and not to publish, copy, reproduce, sell, assign, license, market, transfer or otherwise dispose of, give or disclose such information to third parties, or to use such information for any purposes whatsoever other than as contemplated by this Agreement, unless with prior written consent from Soda. It is understood and agreed that in the event of a breach of confidentiality, damages may not be an adequate remedy and Soda shall be entitled to injunctive relief to restrain any such breach, threatened or actual.

8.Limitation of Liability. The Product is provided without charge and for testing and evaluation purposes only. Accordingly, the total liability of Soda arising out of or related to this Agreement shall not exceed five hundred (500) EUR. In no event shall Soda be liable for any consequential, indirect, incidental or special damages arising hereunder (including, without limitation, damages for loss of profits, business interruption, loss of information or data) however caused and on any theory of liability.

9.Duration. Unless otherwise terminated as specified under this Agreement, the Agreement will automatically terminate upon the expiration of the Program Period. Soda can immediately terminate this Agreement without notice in the event of improper use of the Product or improper disclosure of confidential information. Upon any expiration or termination of this Agreement, Participant shall immediately cease using, and will return to Soda (or, at Soda's request, delete) the Product and all other items in Participant's possession or control that are proprietary to Soda or contain confidential information.

10.Miscellany. The provisions titled Limited Warranty, Confidentiality, Limitation of Liability shall survive termination hereof. This Agreement may not be amended or superseded except by a writing signed by both Participant and Soda, and shall be governed by and construed under the laws of Belgium. Participant shall not assign or otherwise transfer any rights or obligations under this Agreement. All disputes relating to the Agreement, including its interpretation, fall under the exclusive jurisdiction of the courts of the district of Brussels, Belgium.



