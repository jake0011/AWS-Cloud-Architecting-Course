### Question 1

Which scenario describes a challenge to **data velocity**?

* [ ] Regional offices send data in different file formats to an organization's head office.
* [x] **A shopping website collects clickstream data to make personalized recommendations while a user is shopping. When the website is very busy, there is a delay in returning results to customers.**
* [ ] Because it ingests data from regional sales sites, the overnight batch job fails.
* [ ] A sales department wants to use a data source but does not have information about its lineage or the quality of the data.

**Rationale:**
**Data velocity** refers to the **speed at which data is generated, transmitted, and processed**.
The scenario with **clickstream data and delayed recommendations** highlights a **real-time data processing challenge**, which is a classic issue of **high data velocity**.
Other options describe problems related to data variety, volume, or veracity — not velocity.

---
### Question 2

Which statement describes the goal of a **modern data architecture**?

* [ ] Give users the ability to access all of an organization's data through a highly structured data warehouse that provides for fast SQL queries.
* [x] **Give users the ability to access all of an organization's data by integrating a data lake, a data warehouse, and other purpose-built data stores.**
* [ ] Select purpose-built streaming services to make all of the organization's data available for real-time analysis.
* [ ] Select a single ingestion service that can support the data formats, structures, and velocity requirements of all the data sources that will be collected.

**Rationale:**
A **modern data architecture** unifies different storage and analytics services — such as **data lakes, data warehouses, and purpose-built databases** — to allow **seamless data access and analysis** across an organization.
It supports multiple data types (structured, semi-structured, and unstructured) and analytical approaches (batch, real-time, ML) for **scalability and flexibility**.

---
### Question 3

Which analytic workload scenario can be a use case for the **batch ingestion process**?

* [x] **Send sales transaction data from a retailer's website to a central location periodically. Analyze the data overnight, and deliver reports to branches in the morning.**
* [ ] Produce real-time alerts based on log data to identify potential fraud as soon as it occurs.
* [ ] Populate a dashboard with real-time error rates of sensors in a factory.
* [ ] Send small bits of clickstream data at a continuous pace from a retailer's website for immediate analysis.

**Rationale:**
**Batch ingestion** involves collecting and processing data at scheduled intervals — ideal for workloads like **daily sales reporting or overnight analytics**.
It is cost-effective for **non-real-time** use cases where immediate insights are unnecessary, unlike **streaming ingestion**, which handles continuous data for real-time processing.

---
### Question 4

A medical research company has a ribonucleic acid (RNA) sequencing machine that stores its private results to the lab's on-premises network-attached storage. Their data science team wants to ingest these results into their AWS account. How should they ingest this data?

* [x] **Use AWS DataSync to transfer data from the on-premises file store to an Amazon S3 bucket in the data lake.**
* [ ] Use AWS Database Migration Service (AWS DMS) to sync data from the on-premises file store to Amazon S3.
* [ ] Use AWS Glue to connect to the on-premises data and move the data into the pipeline.
* [ ] Use AWS Data Exchange to subscribe to the RNA-sequencing data.

**Rationale:**
**AWS DataSync** is purpose-built for **secure, fast, and automated transfer of large datasets** between on-premises storage and **Amazon S3** (or other AWS storage).
It is ideal for scientific workloads like **RNA sequencing** where large volumes of unstructured data must be efficiently migrated to AWS for analysis.

---
### Question 5

A data engineer has ingested a new JSON file into an Amazon S3 bucket in their data lake. An AWS Glue Data Catalog maintains metadata about data in the lake. Which feature of AWS Glue can the data engineer use to discover the JSON data schema with the fewest steps in a code-free way?

* [ ] Use AWS Glue Studio to transform the data and move it into the data warehouse.
* [x] **Run an AWS Glue crawler on the S3 bucket.**
* [ ] Use AWS Glue Studio to write a script that converts the JSON data to Apache Parquet format.
* [ ] Set up an AWS Glue workflow to orchestrate a set of jobs that transforms the data into an open columnar format.

**Rationale:**
An **AWS Glue crawler** automatically scans data in Amazon S3 (or other sources), **infers the schema**, and **updates the AWS Glue Data Catalog** — all without writing code. This is the **simplest, no-code way** to make newly ingested JSON data queryable and discoverable for analytics.

---
### Question 6

A data pipeline will ingest clickstream data from a shopping website. The data engineer must transform data as it arrives to feed a real-time analytics Amazon OpenSearch Service dashboard. They must also generate a monthly report based on the dashboard. Which configuration meets this need?

* [ ] Use two data pipelines: one to ingest data into Amazon Managed Service for Apache Flink and send it to OpenSearch Service and one to send the streaming data directly from Amazon Kinesis Data Streams to Amazon S3.
* [ ] Use Amazon Kinesis Data Firehose to capture the data and send the data to Amazon S3. Run an AWS Glue crawler to support querying the data for the dashboard.
* [ ] Use Amazon Kinesis Data Streams to capture the data. Use Amazon Managed Service for Apache Flink as a consumer to deliver data to OpenSearch Service. Use Amazon Redshift Spectrum to run SQL queries on the data stream for the monthly report.
* [x] **Use Amazon Kinesis Data Streams to capture the data. Use Amazon Managed Service for Apache Flink to consume and transform data from the stream. Use Amazon Kinesis Data Firehose to deliver transformed data to OpenSearch Service.**

**Rationale:**
This configuration enables **real-time data ingestion, transformation, and analytics**:

* **Amazon Kinesis Data Streams** captures high-velocity clickstream data.
* **Amazon Managed Service for Apache Flink** performs real-time data transformation and enrichment.
* **Amazon Kinesis Data Firehose** delivers transformed data to **Amazon OpenSearch Service** for live dashboards and to **Amazon S3** for long-term storage and monthly reporting.
  This setup minimizes latency and supports both **real-time analytics** and **batch reporting**.

---
### Question 7

Which statement accurately describes a consideration for designing pipeline storage?

* [ ] Choose the storage option that provides the fastest queries regardless of the use case.
* [x] **Archive data out of relational databases into a more cost-efficient storage option.**
* [ ] Choose the lowest cost storage option regardless of the intended use case.
* [ ] Store raw data that will be used for analytics in a data warehouse.

**Rationale:**
When designing data pipelines, it’s important to **optimize storage based on data lifecycle and cost efficiency**. Archiving older or infrequently accessed data from **relational databases** into **low-cost storage** (like Amazon S3 or Glacier) reduces costs while preserving accessibility for analytics or compliance. Data warehouses are typically used for **structured, processed data**, not raw data ingestion.

---
### Question 8

A data engineer is designing a low-cost infrastructure to store data directly from a central repository for both structured and unstructured data. Which option meets the data engineer's needs?

* [ ] Amazon Quantum Ledger Database (Amazon QLDB)
* [ ] AWS Database Migration Service (AWS DMS)
* [ ] Amazon Redshift
* [x] **Amazon S3**

**Rationale:**
**Amazon S3** is the most cost-effective and scalable storage option for **both structured and unstructured data**. It serves as the foundation for **data lakes**, allowing ingestion from multiple sources and integration with AWS analytics and machine learning services. Other options, like Redshift and QLDB, are specialized for structured or transactional workloads and are not optimized for low-cost, large-scale storage.

---
### Question 9

A DevOps engineer is migrating an on-premises Apache Hadoop cluster to AWS. The cluster runs scheduled jobs by using parallel processing. Which AWS service is the MOST appropriate choice?

* [ ] AWS Glue DataBrew
* [x] **Amazon EMR**
* [ ] AWS Glue
* [ ] Amazon Managed Service for Apache Flink**

**Rationale:**
**Amazon EMR (Elastic MapReduce)** is the most suitable service for migrating an **Apache Hadoop cluster** to AWS. It provides a managed big data platform for processing large datasets using open-source frameworks such as **Hadoop, Spark, Hive, and Presto**. EMR supports **parallel and distributed processing**, making it ideal for running **scheduled batch jobs** at scale.
Other services like AWS Glue and Flink serve specific ETL or streaming use cases, while **EMR** directly supports the Hadoop ecosystem and workload migration.

---
### Question 10

A marketing manager quickly needs one-time insights about the number of leads and closed deals across multiple postal codes. Which service would be the MOST cost-effective method to query daily aggregates of sales data stored in Amazon S3?

* [x] **Amazon Athena**
* [ ] Amazon OpenSearch Service
* [ ] Amazon QuickSight
* [ ] Amazon Redshift

**Rationale:**
**Amazon Athena** is a **serverless, pay-per-query service** that allows you to run SQL queries directly on data stored in **Amazon S3**. It is ideal for **ad-hoc, one-time analytics** without requiring infrastructure setup or ongoing cluster costs. The user only pays for the amount of data scanned, making it the **most cost-effective** choice for quick insights.
In contrast, Amazon Redshift is better for persistent data warehousing, and QuickSight is used for visualization (often powered by Athena or Redshift). OpenSearch Service is optimized for text search and log analytics, not SQL-style aggregation.
