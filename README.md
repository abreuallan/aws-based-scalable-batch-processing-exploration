
# AWS Based Scalable Batch Processing and Exploration

This cloud based architecture is meant to be **highly scalable**, allowing for **massive data growth without loss of performance** due to the **distributed nature** of the tools used. All tools referenced are part of the **AWS Suite**.

![Architecture](https://i.imgur.com/fuK2cpm.png)


The **RDS** is our **OLTP** database designed to store the application data.

We also have a **read replica** from the RDS, so that we get much **better read speeds** and don't impact the OLTP executing on the main database.

The **Kinesis** would watch for data changes on the read replica and pass it to the **EMR** for processing. At the same time, **Kinesis Data Firehose** captures the raw data and writes it to an **S3 bucket**.

The EMR runs in **micro batches**(based on the amount of data that has already been collected in Kinesis). It **transforms, aggregates and models** the data using a **Spark job** that can also consume **already stored** S3 data for KPI and metrics building. The resulting data is sent to **Redshift** for storage.

The EMR is also used to **prepare data** for the **machine learning models** that will be built/trained/deployed using **SageMaker**.

Redshift would have the **modeled data with its respective KPIs and metrics** so that it can be queried/explored using **Quicksight or other BI tool** to provide OLAP analysis.

The exploration tool should also be able to directly join Redshift data with S3 stored data through **Athena**, as well as importing other **external datasets** to enrich the analysis, e.g. Excel Sheets, CSV files.
