# Module 6: Adding a Database Layer 

---

## Introduction  
This module focuses on database types and services offered by **Amazon Web Services (AWS).**  


## Module Objectives  
This module prepares you to:  
- Compare database types and services offered by AWS.  
- Explain when to use **Amazon Relational Database Service (Amazon RDS).**  
- Describe the advanced features of **Amazon RDS.**  
- Explain when to use **Amazon DynamoDB.**  
- Identify **AWS purpose-built database services.**  
- Describe how to **migrate data into AWS databases.**  
- Design and deploy a **database server.**  
- Apply the **AWS Well-Architected Framework principles** when designing a database layer.  


## Module Overview  
**Presentation sections include:**  
- Database layer considerations  
- Amazon RDS  
- Amazon RDS Proxy connection management  
- Amazon DynamoDB  
- Purpose-built databases  
- Migrating data into AWS databases  
- Applying AWS Well-Architected Framework principles to the database layer  

**Demo:**  
- Amazon RDS Automated Backup and Read Replicas  

**Knowledge checks:**  
- 10-question knowledge check  
- Sample exam question  


## Hands-on Labs in this Module  
- **Guided Lab:** Creating an Amazon RDS Database  
- **Challenge (Café) Lab:** Migrating a Database to Amazon RDS  

Labs are supported with detailed instructions in the student guide and lab environment.  


## Cloud Architect Perspective  
As a cloud architect designing a database layer, you should:  
- Evaluate available **database options** before selecting a solution to optimize performance.  
- **Secure infrastructure** to ensure durability and protect data from threats.  

Keep in mind:  
- Work backward from the **business need** to design the most effective architecture.  
- Use the **café scenario** in the course as a guide to apply these principles in a real-world example.  

---

## Section 2: Database Layer Considerations

### Database considerations
- **Scalability**: How much throughput is required? Can it scale effectively?  
- **Storage requirements**: Will the DB store gigabytes, terabytes, or petabytes?  
- **Data characteristics**: Data model (relational, semi-structured, time-series, etc.), access patterns, latency needs.  
- **Durability**: Availability, recoverability, and regulatory obligations.  

Proper scaling is key:  
- Underprovisioning → application failure.  
- Overprovisioning → wasted costs (violates cost-optimization).  
- Ideal → handle throughput at launch, scale up later as needed.  

### Storage requirements
- Consider total data size and type of workload.  
- Different DB designs support different capacities.  
- Some fit traditional apps; others suit caching or session management.  

### Data characteristics
- Define what needs storing, retrieving, analyzing.  
- Includes model, access needs, latency demands, record sizes.  

### Durability
- Durability = data not lost.  
- Availability = access when needed.  
- Critical data → redundant copies across regions.  
- Trade-off: higher cost for higher durability.  
- Consider regulatory compliance (e.g., regional data laws).  


## Relational and Non-Relational Databases

### Relational Databases
- **Structure**: Tables (rows/columns), strict schema.  
- **Benefits**: Ease of use, integrity, storage efficiency, SQL support.  
- **Use case**: Migrating on-premises workloads, online transactional processing.  
- **Optimization**: Structured data, supports joins.  
- **Transactions**: ACID-compliant (Atomic, Consistent, Isolated, Durable).  

### Non-Relational Databases
- **Structure**: Flexible (key-value, document, graph, in-memory).  
- **Benefits**: Flexibility, scalability, high performance.  
- **Use case**: Caching layers, JSON storage, millisecond data retrieval.  
- **Optimization**: Handles structured, semi-structured, unstructured data.  
- **Schema**: Each object can differ.  

Both types can scale:  
- Relational (e.g., Amazon RDS) → read replicas.  
- Non-relational (e.g., Redis) → bigger instances, horizontal scaling.  


### Amazon Database Options

- **Relational**: Amazon RDS  
  - Managed service, supports engines: Aurora (MySQL/PostgreSQL), MySQL, MariaDB, PostgreSQL, Oracle, SQL Server.  
  - Use cases: ERP, CRM, ecommerce.  

- **Non-relational**:  
  - **Amazon DynamoDB** → key-value  
  - **Amazon Neptune** → graph  
  - **Amazon ElastiCache** → in-memory  


### Managed Database Services vs Other Hosting

| Task                        | On-Premises | Amazon EC2 | Managed Service |
|-----------------------------|-------------|------------|-----------------|
| Power, HVAC, Networking     | x           |            |                 |
| Rack and Stack              | x           |            |                 |
| Server Maintenance          | x           |            |                 |
| OS Installation             | x           |            |                 |
| OS Patches                  | x           | x          |                 |
| Database Installation       | x           | x          |                 |
| Database Patches            | x           | x          |                 |
| Database Backups            | x           | x          |                 |
| High Availability           | x           | x          |                 |
| Scaling                     | x           | x          |                 |
| Application Optimization    | x           | x          | x               |

- **On-premises** → full responsibility.  
- **Amazon EC2** → AWS manages hardware, you manage OS, DB, scaling.  
- **Managed services (RDS, Aurora)** → AWS handles admin tasks; you optimize queries.  


### Database Capacity Planning

Steps:  
1. Analyze current capacity.  
2. Predict future requirements.  
3. Decide between **vertical scaling** or **horizontal scaling**.  

- **Vertical scaling**: Add more resources to one server (CPU, RAM, storage). Usually downtime required.  
- **Horizontal scaling**: Add more servers, distribute load. Usually no downtime.  

### Key Takeaways
- Consider scalability, storage, data type, cost, durability, compliance.  
- **Relational DBs**: strict schema, vertical scaling, structured data, ACID transactions.  
- **Non-relational DBs**: flexible schema, horizontal scaling, high performance for semi/unstructured data.  
- **Managed AWS DBs** reduce admin responsibility; you focus on query and app optimization.  

---

## Section 3: Amazon RDS: Adding a Database Layer

This section looks at the characteristics and use cases of Amazon RDS and Aurora.  


### Amazon Relational Database Service (Amazon RDS)

Amazon RDS is a managed relational database service that helps you deploy and scale relational databases.  

- Supports multiple database engines  
- Uses Amazon Elastic Block Store (EBS) volumes for database and log storage  

Why choose a managed relational database in AWS?  
Because AWS takes care of many tedious tasks like provisioning, patching, backup, recovery, failure detection, and repair.  

With RDS, you don’t have to provision infrastructure or maintain software. It automates routine admin tasks and supports these engines:  

- Aurora (MySQL-compatible)  
- Aurora (PostgreSQL-compatible)  
- RDS for MySQL  
- RDS for MariaDB  
- RDS for PostgreSQL  
- RDS for Oracle  
- RDS for SQL Server  

You can also scale the storage capacity allocated to your DB instance.  


### Benefits of Amazon RDS

- **Lower administrative burden** – No need to provision infrastructure or install software. Single console + API for management.  
- **Highly scalable** – Easily scale compute and storage with clicks or API calls. Multiple instance types to suit workloads.  
- **Available and durable** – Replication, Multi-AZ deployments, automatic failover, and read replicas for scaling read-heavy workloads.  
- **Secure and compliant** – Run in Amazon VPC, isolate instances, control access, and enable encryption (at rest & in transit). Supports compliance like HIPAA.  


### Amazon RDS Database Architecture

- Seven database engine options: Aurora (MySQL, PostgreSQL), RDS for MySQL, PostgreSQL, MariaDB, Oracle, SQL Server.  
- RDS instances are isolated environments, each able to host multiple user databases.  
- Uses EBS volumes for database + log storage.  
- Provides managed deployment of relational database systems with EC2 instances for compute capacity.  


### Database Layer Architecture Diagram

The architecture includes:  
- An EC2 instance and an Amazon RDS instance inside a Virtual Private Cloud (VPC).  
- A VPC is an isolated virtual environment for AWS resources.  
- You’ll get hands-on by creating a DB layer with Amazon RDS and choosing a database engine during launch.  


### Aurora

Aurora is a relational database built for the cloud, fully compatible with MySQL and PostgreSQL, and managed by RDS.  

- Up to 5x faster than MySQL and 3x faster than PostgreSQL  
- 1/10 the cost of commercial DBs  
- Automates admin tasks (provisioning, patching, backups)  
- Distributed, fault-tolerant storage, auto scales up to 64 TB  
- Up to 15 low-latency read replicas, point-in-time recovery, continuous S3 backup, and multi-AZ replication  


### Aurora Database Clusters

- **Aurora Cluster Volume** – A virtual volume spanning multiple AZs, each holding a copy of DB cluster data.  
- **Primary instance** – Supports read/write, handles data modifications. One per cluster.  
- **Aurora replicas** – Up to 15 per cluster, support read-only ops. Located in separate AZs for high availability. Automatic failover to a replica if primary fails.  


### Aurora Serverless

Aurora Serverless is an on-demand, auto-scaling configuration for Aurora.  

- **Features**: hands-off capacity management, fine-grained scaling  
- **Best for**: variable workloads, new applications, development/testing, capacity planning  

Aurora Serverless v2 can mix with provisioned instances. It scales closely to demand, reducing admin overhead.  


### Amazon RDS Use Case: Banking Transactions

Amazon RDS works well for Online Transaction Processing (OLTP).  
Example: banking transactions stored in Aurora DB, accessed by EC2-hosted applications.  

Transactions include:  
- Deposits  
- Withdrawals  
- Interest payments  

Each entry has unique ID, date, description, type, and amount.  


### Amazon RDS Instance Types and Sizing

RDS provides several families of instance types:  

- **General purpose** – T4g, T3, M6g, M5 (good for CPU-intensive or moderate workloads)  
- **Memory-optimized** – R6g, R5, X2g, X1E (good for query-heavy workloads and high connections)  

Upgrade strategy:  
- If CPU is limited → move from `m6g.large` to `m6g.xlarge`  
- If memory is limited → move from `r6g.large` to `r6g.xlarge`  


### Amazon RDS Security Best Practices

- Run DB in a private VPC for network control  
- Use IAM policies for permissions  
- Apply security groups for allowed IPs/instances  
- Use SSL/TLS for connections  
- Encrypt DB instances + snapshots with AWS KMS keys (AES-256)  
- Apply DB engine’s own security features for login/access  

*Remember:* Security is a shared responsibility — AWS secures the infrastructure, while you secure access to your databases.  


### Key Takeaways: Amazon RDS

- RDS is a managed relational database service supporting popular engines.  
- Aurora is a cloud-native, managed DB engine with Serverless option.  
- RDS offers different instance types for varied workloads.  
- Performance, scalability, failover, storage, high availability, and backup needs determine the right DB engine.  


---

## Section 4: Amazon RDS Proxy Connection Management

### Amazon RDS Proxy
Amazon RDS Proxy is a fully managed, highly available database proxy for Amazon RDS.  
It supports:
- Aurora (MySQL & PostgreSQL compatibility)  
- RDS for MariaDB, MySQL, PostgreSQL, SQL Server  

**Benefits**:  
- **Scalable**: Pools and shares connections for better scaling.  
- **Resilient**: Reduces failover times by up to 66% for Multi-AZ RDS.  
- **Secure**: Enforces IAM authentication and integrates with AWS Secrets Manager.  

No extra infrastructure is needed to use RDS Proxy.

### Connection Pooling (Scalability)
- **How it works**:  
  - Proxy sits between application and database.  
  - Applications may open thousands of connections, but many remain idle.  
  - Proxy reuses idle connections, reducing DB load.  

- **When to use**:  
  - DB hitting "too many connections" errors.  
  - Apps with frequent open/close connection patterns.  
  - SaaS/e-commerce apps with long-lived, low-latency connections.  

### Seamless and Fast Failover (Availability)
- **Failover process**:  
  1. Proxy detects failover immediately.  
  2. Preserves idle connections and queues new transactions.  
  3. When standby DB is ready, queued transactions are executed.  

- **Result**:  
  - Apps remain resilient to DB failures.  
  - Failover times reduced by bypassing DNS cache.  

### Streamlined Authentication (Security)
- Proxy enforces **IAM authentication** and uses **Secrets Manager**.  
- **Benefits**:  
  - Removes hard-coded passwords.  
  - Centralizes authentication policies.  
  - Lambda functions can use IAM tokens.  

**Authentication flow**:  
1. Application requests token from IAM.  
2. IAM returns token to ECS/Lambda.  
3. App connects to Proxy with token.  
4. Proxy retrieves DB credentials from Secrets Manager.  
5. Proxy forwards request to DB.  

### Backing Up Data in RDS
Amazon RDS provides two backup types:  

| Feature              | Automated Backups              | Database Snapshots            |
|----------------------|--------------------------------|-------------------------------|
| **Use Case**         | Point-in-time restore          | Restore to known state        |
| **Frequency**        | Daily + logs every 5 min       | User-initiated                |
| **Retention**        | 7–35 days (auto-deletion)      | Until manually deleted        |
| **Sharing**          | Not shareable directly         | Shareable across accounts     |

Both are stored in **Amazon S3**.  
Snapshots can be copied across Regions/accounts.

### Cross-Region Backups
- Snapshots & logs can be replicated to another Region for **DR**.  
- Options:  
  - Automated/manual snapshot copy.  
  - Cross-Region read replicas (for DR, scaling reads, migration).  

### Encryption for Backups
- **At rest**: Encrypted with AWS KMS.  
- **In transit**: Protected with SSL/TLS.  

**Encrypting unencrypted DB**:  
1. Take snapshot.  
2. Copy with encryption enabled.  
3. Restore snapshot to new encrypted DB.  

### Demo (Practical)
Demo tasks:  
- Create a backup schedule.  
- Create a read replica.  

(Demo video available in course.)

### Key Takeaways
- RDS Proxy improves **scalability, availability, security**.  
- **Multi-AZ** → synchronous replication → high availability.  
- **Read replicas** → asynchronous replication → high scalability.  
- **Backups**: automated + snapshots.  
- **Encryption** protects data at rest & in transit.  


---

# Section 5: Amazon DynamoDB
This section describes DynamoDB, its features, and how secondary indexing is used to derive insights from data with maximum flexibility.

### DynamoDB
DynamoDB is a fully managed, serverless, NoSQL database. It supports key-value and document data models. It has a flexible schema, so each item can have many different attributes. DynamoDB can provide consistent response times in the single-digit millisecond range. The database automatically scales tables to adjust for capacity and maintains performance with zero administration. DynamoDB helps secure your data with encryption and continuously backs up your data for protection.

### DynamoDB Use Cases
- **Developing software applications**: Build internet-scale applications that support user-content metadata and caches that require high concurrency and connections for millions of users and millions of requests per second.
- **Creating media metadata stores**: Scale throughput and concurrency for media and entertainment workloads, such as real-time video streaming and interactive content, and deliver lower latency with multi-Region replication across AWS Regions.
- **Scaling gaming platforms**: Build out your game platform with player data, session history, and leaderboards for millions of concurrent users.

### DynamoDB Features
- **Serverless performance with limitless scalability**: DynamoDB offers global and local secondary indexes to query the data in the table by using an alternate key. Amazon DynamoDB Streams records a time-ordered sequence of every item-level change in near-real time, making it ideal for event-driven architecture applications. Global tables provide multi-active replication of your data across your choice of AWS Regions for single-digit millisecond read and write performance.
- **Built-in security and reliability**: DynamoDB encrypts all customer data at rest by default. Point-in-time recovery (PITR) protects data from accidental write or delete operations. DynamoDB uses IAM to authenticate, create, and access resources with fine-grained access control.

### Amazon DynamoDB Data Structure
- **Table**: A collection of data containing zero or more items, unique to an account ID and a region.
- **Item**: A group of attributes that is uniquely identifiable among all of the other items. Each attribute consists of a key-value pair.
- **Attribute**: A fundamental data element.
- **Simple primary key**: Composed of one attribute known as the partition key.
- **Composite primary key**: Composed of the partition key and the sort key, which uniquely identify an item.
- **Sort key**: Optional, denotes how items that share the same partition key are sorted within the partition.

## Amazon DynamoDB Sample Base Table
| Partition Key | Sort Key                | Temperature | Error Status |
|---------------|-------------------------|-------------|--------------|
| Device ID: 1  | Timestamp: 2023-11-20 15:42:00 | 41.9        | Low          |
| Device ID: 1  | Timestamp: 2023-11-20 15:42:30 | 42          |              |
| Device ID: 1  | Timestamp: 2023-11-20 15:43:00 | 39          | Low          |
| Device ID: 2  | Timestamp: 2023-11-20 15:42:00 | 47          | Low          |
| Device ID: 2  | Timestamp: 2023-11-20 15:42:30 | 49          | High         |
| Device ID: 2  | Timestamp: 2023-11-20 15:43:00 | 46.9        |              |

This sample base table is used for a query to read the attributes associated with a device. Alternatively, the whole table can be read to get all device readings with a table scan.

## Alternate Schema Using a Global Secondary Index
| Partition Key | Sort Key                | Temperature | Error Status | Timestamp                |
|---------------|-------------------------|-------------|--------------|--------------------------|
| Device ID: 1  | Timestamp: 2023-11-20 15:42:00 | 41.9        | Low          |                          |
| Device ID: 1  | Timestamp: 2023-11-20 15:42:30 | 42          |              |                          |
| Device ID: 2  | Timestamp: 2023-11-20 15:42:30 | 49          | High         |                          |
| Device ID: 2  | Timestamp: 2023-11-20 15:43:00 | 46.9        |              |                          |

| **Global Secondary Index (GSI)** [Eventual consistency] | | | | |
| Temperature: 49 | Device ID: 2          |             | High         | 2023-11-20 15:42:30      |

DynamoDB creates a read-only copy of the base table in which you can pivot the data around different partition and sort keys. Data becomes eventually consistent with the base table, usually within 1 second.

## Alternate Schema Using a Local Secondary Index
| Partition Key | Sort Key                | Temperature | Error Status | Timestamp                |
|---------------|-------------------------|-------------|--------------|--------------------------|
| Device ID: 1  | Timestamp: 2023-11-20 15:42:00 | 41.9        | Low          |                          |
| Device ID: 1  | Timestamp: 2023-11-20 15:42:30 | 42          |              |                          |
| Device ID: 2  | Timestamp: 2023-11-20 15:42:30 | 49          | High         |                          |
| Device ID: 2  | Timestamp: 2023-11-20 15:43:00 | 46.9        |              |                          |

| **Local Secondary Index (LSI)** [Eventual or strong consistency] | | | | |
| Device ID: 2  | Error Status: High      | 49          |              | 2023-11-20 15:42:30      |

The partition key stays the same as the base table, but the sort key can be different. LSIs must be created along with the table. You can choose either eventual consistency or strong consistency.

### Alternate Schema Using a Local Secondary Index
- **Base table**: Same as above.
- **Local Secondary Index (LSI)** [Eventual or strong consistency]:
  - Partition key = Device ID: 2, Sort key = Error Status: High, Attribute = Temperature: 49, Timestamp: 2023-11-20 15:42:30
The partition key stays the same as the base table, but the sort key can be different. LSIs must be created along with the table. You can choose either eventual consistency or strong consistency.

### Multi-Region Replication: Amazon DynamoDB Global Tables
A global table is a collection of one or more replica tables, all owned by a single AWS account. Each replica stores the same set of data items. Global tables replicate your Amazon DynamoDB tables automatically across your choice of AWS Regions for fast, local read and write performance. They eliminate the work of replicating data between regions and resolving update conflicts.

### DynamoDB Security Best Practices
- **Preventative**:
  - Use IAM roles to authenticate access.
  - Use IAM policies for DynamoDB base authorization.
  - Use IAM policy conditions for fine-grained access control.
  - Use a VPC endpoint and policies to access DynamoDB.
- **Detective**:
  - Use AWS CloudTrail to monitor AWS managed AWS KMS key usage.
  - Monitor DynamoDB operations by using CloudTrail.
  - Monitor DynamoDB configuration with AWS Config.
  - Monitor DynamoDB compliance with AWS Config rules.

### Key Takeaways: Amazon DynamoDB
- DynamoDB is a fully managed, serverless, NoSQL database that supports key-value and document data models.
- DynamoDB offers global and local secondary indexes to provide flexibility on how to access your data.
- Global tables provide a multi-Region, multi-active database for fast performance for global applications.
- DynamoDB encrypts all user data at rest stored in tables, indexes, and streams. Preventative and detective security best practices help ensure data protection.

---

## Seciton 6: Purpose-Built Databases.
This section describes the relational database offering Amazon Redshift and six non-relational database options that are purpose-built for specific use cases.

### The Evolution of Purpose-Built Databases
Application architectures have changed quite a bit in recent decades. Each of the major shifts focused on splitting up your application to improve scaling and resiliency. Parallel to the evolution of application architecture is the evolution of how you store and access data in increasingly purpose-built solutions. 
| Opportunity | Database Evolution | AWS Examples |
|-------------|--------------------|--------------|
| Improve on hierarchical databases' limited abilities to define relationships among data. | Relational | Amazon RDS |
| Improve performance by separating read-heavy reporting from an application's transactional database. | Data warehouse/OLAP | Amazon Redshift |
| Analyze the more varied types of data being generated in large amounts on the internet. | Non-relational | DynamoDB |
| Take advantage of cloud computing's freedom to scale data stores up and down based on actual usage, and the ease of connecting different microservices to purpose-built data stores. | Purpose-built: Document, Wide-column, In-memory, Graph, Timeseries, Ledger | Fully managed database services: Amazon DocumentDB, Amazon Keyspaces, MemoryDB, Neptune, Timestream, Amazon QLDB |

Relational databases replaced hierarchical ones, which had limited abilities to define relationships among data. Data warehouses evolved as a way to separate operational reporting, which requires read-heavy querying across a full dataset, from the application's transactional database, which is focused on fast reads and writes in a much smaller set of records. Data warehouses are still relational databases, but they are online analytical processing (OLAP) databases that have been optimized for reporting and analytics. As the internet era took off, organizations started to get more varied types of data, which they wanted to be able to analyze. This new variety of data was less structured and needed less rigid rules. This change in the types of data led to the development of the first non-relational databases in the late 90s. Cloud computing gave organizations the freedom to scale data stores up and down based on actual usage. Coupled with a move toward microservices, the flexibility of the cloud made it attractive to connect different application components to different databases rather than relying on a single multipurpose one. Additional non-relational purpose-built databases emerged to give developers the ability to optimize the storage that is connected to a component to match the data type and processing of that component.

### Amazon Redshift
Amazon Redshift is a fully managed, cloud-based data warehousing service designed to handle petabyte-scale analytics workloads. It achieves efficient storage and optimum query performance through a combination of massively parallel processing, columnar data storage, and very efficient, targeted data compression encoding schemes. Amazon Redshift manages all of the work of setting up, operating, and scaling a data warehouse. Amazon Redshift Serverless adjusts capacity in seconds to deliver consistently high performance. 
- **Use cases**:
  - Automatically create, train, and deploy machine learning models to improve financial and demand forecasts.
  - Securely share data among accounts, organizations, and partners while building applications on top of third-party data.
  - Increase developer productivity by getting simplified data access without configuring drivers and managing database connections.

### AWS Fully Managed Purpose-Built Database Options
| Database | Type |
|----------|------|
| Amazon DocumentDB (with MongoDB compatibility) | Document database |
| Amazon Keyspaces (for Apache Cassandra) | Cassandra workloads |
| Amazon MemoryDB for Redis | In-memory workloads |
| Amazon Neptune | Graph database |
| Amazon Timestream | Timeseries database |
| Amazon Quantum Ledger Database (Amazon QLDB) | Ledger database |

All of these databases are fully managed cloud services that help you limit the time and cost of experimenting and maintaining different types of databases.

### Matching a Database to Your Business Need
- **Suitable workloads**: Analyze your workload requirements to see if they match the database's capabilities.
- **Data model**: Understand the characteristics of the data model that you would need to use with the database.
- **Features and benefits**: Familiarize yourself with key features and configuration options to optimize performance.
- **Common use cases**: Review common use cases to find reference architectures and examples.

### Amazon DocumentDB
- **Suitable workloads**: Require a flexible schema for fast, iterative development. Need to store data that has different attributes and data values.
- **Data model**: Uses a document data model. Stores and quickly accesses documents. Queries on any attribute. Documents are typically stored as JSON files.
- **Features and benefits**: Is a MongoDB-compatible, JSON document database. Is good for complex documents that are dynamic and require one-time querying, indexing, and aggregations. Native integrations with AWS Database Migration Service (AWS DMS) make integration possible with virtually no downtime.
- **Common use cases**: Content management system (CMS). Customer profiles. Storage and management of operational data from any source.

### Amazon Keyspaces
- **Suitable workloads**: Fast querying capability of high volumes of data. Scalability and consistent performance on heavy write loads.
- **Data model**: Is a wide column data model. Stores data in flexible columns, which permits data to evolve over time. Partitions across distributed database systems.
- **Features and benefits**: Scalable, highly available, and managed Apache Cassandra-compatible database service. You can use CQL API code, Cassandra drivers, and developer tools without having to manage servers.
- **Common use cases**: Process data at high speeds for applications that require millisecond latency: industrial equipment maintenance, trade monitoring, route optimization.

### Amazon MemoryDB
- **Suitable workloads**: Latency-sensitive workloads. High request rate. High data throughput. No data loss.
- **Data model**: Is an in-memory database service. Relies primarily on memory for data storage. Minimizes response time by eliminating the need to access disks.
- **Features and benefits**: Stores an entire dataset in memory and leverages a distributed transactional log to provide in-memory speed and consistency, data durability, and recoverability.
- **Common use cases**: Caching. Game leaderboards. Bank user transactions.

### Amazon Neptune
- **Suitable workloads**: Find connections or paths in data. Combine data with complex relationships across data silos. Navigate highly connected datasets, and filter results based on certain variables.
- **Data model**: Is a graph data model. Stores data and the relationship of that data to other data as equally important to the data itself. Quickly creates and navigates relationships between data.
- **Features and benefits**: Supports graph applications that require high throughput and low latency graph queries. Creates and navigates data relations quickly.
- **Common use cases**: Recommendation engines. Fraud detection. Knowledge graphs. Drug discovery. Social networking.

### Amazon Timestream
- **Suitable workloads**: Identify patterns and trends over time. Determine value or performance over time. Rely on efficient data processing and analytics. Require ease of data management.
- **Data model**: Is a timeseries data model. Is a sequence of data points recorded over a time interval for measuring events that change over time. Provides the ability to collect, store, and process data sequenced by time.
- **Features and benefits**: Simplifies timeseries data access. Has built-in timeseries functions for quick analysis of timeseries data by using SQL.
- **Common use cases**: Identify trends from data generated by Internet of Things (IoT) applications. Improve performance by analyzing web traffic data for your applications in real time.

### Amazon QLDB
- **Suitable workloads**: Maintain an accurate history of application data. Track the history of financial transactions. Verify the data lineage of claims.
- **Data model**: Is a ledger database. Provides an immutable and verifiable history of all changes to application data.
- **Features and benefits**: Provides a transparent, immutable, and cryptographically verifiable transaction log. Provides built-in data integrity. Provides a consistent event store.
- **Common use cases**: Storing financial transactions. Maintaining claim history.

### Key Takeaways: Purpose-Built Databases
- Amazon Redshift is a cloud-based data warehousing service.
- Amazon DocumentDB is a MongoDB-compatible, JSON document database.
- Amazon Keyspaces is a wide-column database service that is compatible with Apache Cassandra.
- MemoryDB is a durable in-memory database service.
- Neptune is a high-performance graph database engine.
- Timestream is a purpose-built database for timeseries data.
- Amazon QLDB is a ledger database that maintains a verifiable log of data changes.

---
## Section 7: Migrating Data into AWS Databases: Adding a Database Layer
This section describes how AWS Database Migration Service (AWS DMS) provides migration and replication solutions.

### AWS DMS
AWS DMS is a managed migration and replication service. It helps move existing database and analytics workloads to and within AWS. It supports most widely used commercial and open source databases. It replicates data on demand or on a schedule to replicate changes from a source. AWS DMS is a web service that you can use to migrate data from a source data store to a target data store. These two data stores are called endpoints. You can migrate between source and target endpoints that use the same database engine (homogenous migration) and migrate between source and target endpoints that use different database engines (heterogenous migration). One of your endpoints must be on an AWS service. AWS DMS supports a range of relational, NoSQL, analytics, and data warehouse sources and targets. The source database remains fully operational during the migration, minimizing downtime to applications that rely on the database. You can continuously replicate data with low latency from any supported source to any supported target by using AWS DMS. For example, you can replicate from multiple sources to Amazon S3 to build a highly available and scalable data lake solution. You can also consolidate databases into a petabyte-scale data warehouse by streaming data to Amazon Redshift. You cannot use AWS DMS to migrate from an on-premises database to another on-premises database. Supported source databases include Oracle, Microsoft SQL Server, MySQL, MariaDB, PostgreSQL, IBM Db2 LUW, SAP, MongoDB, and Aurora. Target database engines include Oracle, Microsoft SQL Server, PostgreSQL, MySQL, Amazon Redshift, SAP ASE, Amazon S3, and DynamoDB.

### AWS DMS Homogeneous Migration
Homogeneous data migrations in AWS DMS simplify the migration of self-managed, on-premises databases or cloud databases to the equivalent engine on Amazon RDS or Aurora. For example, you can use homogeneous data replications to migrate an on-premises PostgreSQL database to Amazon RDS for PostgreSQL or Aurora PostgreSQL. Homogeneous data migrations are serverless, which means that AWS DMS automatically scales the resources that are required for your migration. For homogeneous data replications, AWS DMS uses native database tools to provide high-performing like-to-like migrations. At a high level, homogeneous data migrations operate with instance profiles, data providers, and replication projects. This example shows that the database engine is the same for the source database (on-premises MySQL) and the target database (Amazon RDS database running MySQL). 1. An instance profile specifies network and security settings for the managed environment where your migration project runs. When you create a migration project with the compatible source and target data providers of the same type, AWS DMS deploys a managed environment where your data migration runs. 2. Next, AWS DMS connects to the source data endpoint, reads the source data, dumps the files on the disk, and restores the data by using native database tools.

### Tools for Heterogeneous Database Migrations
| Tool | Description |
|------|-------------|
| AWS DMS Fleet Advisor | Automatically inventories and assesses on-premises database and analytics server fleet. |
| AWS Schema Conversion Tool (AWS SCT) | Converts your source schema and SQL code into an equivalent target schema and SQL code. |
| AWS DMS Schema Conversion | Is a centrally managed service for schema assessment and conversion available within AWS DMS workflows. |

Heterogenous migration is migrating between source and target endpoints that use different database engines. There are multiple solutions for simplifying database migrations by automating schema analysis, recommendations, and conversion at scale. Three of them are explained here. AWS DMS Fleet Advisor is a database discovery tool that automatically inventories and assesses your on-premises database and analytics server fleet and identifies potential migration paths. AWS DMS Fleet Advisor can recommend potential database engine and instance options for migration to AWS. This tool delivers results in a few hours instead of weeks or even months without using third-party tools or hiring migration experts. AWS offers two schema conversion solutions to make heterogeneous database migrations predictable, fast, and secure: 1. Download the AWS Schema Conversion Tool (AWS SCT) software to your local drive. 2. Log in to the AWS DMS console to initiate the AWS DMS Schema Conversion workflow for a fully managed experience. Both options will automatically assess and convert the source database schema and a majority of the database code objects to a format compatible with the target database. Any objects that cannot be automatically converted are clearly marked as action items with prescriptive instructions on how to convert so that they can be manually converted to complete the migration.

### AWS DMS Heterogeneous Migration with AWS SCT
Aurora cluster. This example illustrates an AWS DMS heterogenous migration by using AWS Schema Conversion Tool (SCT): 1. Convert: The AWS SCT works in conjunction with AWS DMS when you are moving your source database to a different target database to convert all schema and code objects to the target engine. The AWS SCT automatically converts your source database schemas and most of the database code objects to a format compatible with the target database. The AWS SCT is a standalone application that you need to download to your local drive. Alternatively the AWS DMS replication task can use the AWS DMS Schema conversion feature instead of AWS SCT. 2. Migrate schema and code: You create an AWS DMS migration by creating the necessary replication instance, endpoints, and tasks in an AWS Region. An AWS DMS replication instance is a managed EC2 instance that hosts one or more replication tasks. One or more migration tasks are created to migrate data between the source and target data stores.

### AWS DMS Replicates Data from a Database into a Data Lake
Another use case is data replication into a data lake. In this higher education use case, AWS DMS is used to replicate data from a database into an Amazon S3 data lake: 1. Student Information System data exists in an on-premises database. This data needs to be ingested into the data lake for analytics. 2. An AWS DMS source endpoint is connected to the SIS data. 3. An AWS DMS replication task is run to replicate data from the demo SIS database. 4. An AWS DMS destination endpoint connects the replication task to the Amazon S3 raw data bucket. 5. The data can be accessed by analysis and visualization services to derive insight from the student information data.

### Key Takeaways: Migrating Data into AWS Databases
- AWS DMS is a managed migration and replication service that helps you move your databases and analytics workloads to AWS quickly and securely.
- You can migrate between source and target endpoints that use the same database engine (homogenous migration) and migrate between source and target endpoints that use different database engines (heterogenous migration).
- AWS SCT and AWS DMS Schema Conversion are tools for schema and code conversion.

---

# Section 8: Applying AWS Well-Architected Framework Principles to the Database Layer.
This section looks at how to apply the AWS Well-Architected Framework principles to your database layer.

## AWS Well-Architected Framework Database Pillars
The AWS Well-Architected Framework has six pillars, and each pillar includes best practices and a set of questions that you should consider when you architect cloud solutions. This section highlights a few best practices from the pillars that are most relevant to this module. As you make connections between the AWS Well-Architected Framework pillars and the database layer, consider your role as a cloud architect who is adding a database layer: You need to evaluate the available database options before selecting a data management solution so that you can optimize performance. You need to secure your infrastructure effectively so that data is durable and safe from threats.

## Best Practice Approach: Architecture Selection
Use a data-driven approach for architectural choices. Evaluate how trade-offs impact customers and architecture efficiency. Database architecture selection is an important best practice under the performance efficiency pillar. The optimal database solution for a system varies based on requirements for availability, consistency, partition tolerance, latency, durability, scalability, and query capability. Many systems use different database solutions for various sub-systems and allow different features to improve performance. Selecting the wrong database solution and features for a system can lead to lower performance efficiency. Three important best practices related to database architecture selection include understanding data characteristics, evaluating available options, and choosing data storage based on access patterns.
- **Evaluate how trade-offs impact customers and architecture efficiency**: When evaluating performance-related improvements, determine which choices impact your customers and workload efficiency. For example, if using a key-value data store increases system performance, it is important to evaluate how the eventually consistent nature of this change will impact customers. When selecting and implementing a data management solution, you must ensure that the querying, scaling, and storage characteristics support the workload data requirements. Before implementing a solution, learn how each available option matches your data models and use case.
- **Use a data-driven approach for architectural choices**: Define a clear, data-driven approach for architectural choices to verify that the right cloud services and configurations are used to meet your specific business needs. Use the access patterns of the workload and requirements of the applications to decide on optimal data services and technologies to use. Understanding when to use read replicas, global tables, data partitioning, and caching will help you decrease operational overhead and scale based on your workload needs. For example, you should identify and document the anticipated growth of the data and traffic. With that pattern in mind, consider available database options and configurations.
- **Document workload data characteristics** with enough detail to facilitate the selection and configuration of supporting database solutions, and provide insight into potential alternatives. Once you have considered the workload data characteristics enough to identify alternatives, evaluate and verify your selections and the available database configuration options that best suit your specific workload. Run load tests to identify your key performance metrics and bottlenecks. Use these characteristics and metrics to evaluate database options and experiment with different configurations. Don't increase instance size to improve performance without looking at the impact of available configuration options. Evaluate the acceptable query times to ensure that the selected database options can meet the requirements. For example, after determining the extent to which your workload is read-heavy, you can evaluate solutions that are available for offloading reads such as read replicas or caching.
- **Factor cost into architectural decisions**: Factor cost into your architectural decisions to improve resource utilization and performance efficiency of your cloud workload. When you are aware of the cost implications of your cloud workload, you are more likely to leverage efficient resources and reduce wasteful practices. In this module, you learned that AWS has a variety of database solutions so that you can select the one that meets the workload requirements and data models for your use case. For example, use Amazon RDS for relational data, Amazon Redshift for a data warehouse, DynamoDB for key-value data that requires very low latency at very high scale, or one of the other purpose-built databases that are designed for specific workloads. You also learned about the configuration options that optimize performance for your selected database. For example with Amazon RDS, you can configure the instance type and size suited to your workload. You can configure high availability and implement read replicas. You can use RDS Proxy to scale connection pools. DynamoDB automatically scales horizontally, and you learned how secondary indexes and global tables can improve performance based on your table structures and access patterns.

## Best Practice Approach: Data Protection – Protecting Data at Rest
Implement secure key management. Enforce encryption at rest. Protecting the data residing in your database storage is a critical part of the security pillar. Encryption maintains the confidentiality of sensitive data in the event of unauthorized access or accidental disclosure. Protecting your data at rest reduces the risk of unauthorized access when encryption and appropriate access controls are implemented.
- **Implement secure key management**: By defining an encryption approach that includes the storage, rotation, and access control of keys, you can help provide protection for your content against unauthorized users and against unnecessary exposure to authorized users. AWS KMS helps you manage encryption keys and integrates with many AWS services. This service provides durable, secure, and redundant storage for your AWS KMS keys.
- **Enforce encryption at rest**: You should ensure that the only way to store data is by using encryption. AWS KMS integrates seamlessly with many AWS services to help you encrypt all your data at rest. The following are examples of these best practices you learned about in this module: Amazon databases such as Amazon RDS and DynamoDB use AWS KMS to secure data. DynamoDB encrypts all user data at rest stored in tables, indexes, streams, and backups by using encryption keys stored in AWS KMS. Amazon RDS encrypts your databases by using keys that you manage with AWS KMS. For an Amazon RDS encrypted database instance, data stored at rest in the underlying storage is encrypted, as are all logs, backups, and snapshots. After your data is encrypted, Amazon RDS handles the authentication of access and the decryption of your data transparently with minimal impact on performance.

## Best Practice Approach: Cost-Effective Resources – Select the Correct Resource Type, Size, and Number
Select the resource type, size, and number based on data. By selecting the best database type, size, and number of resources, you meet the technical requirements with the lowest cost resource. Right-sizing activities takes into account all of the resources of a workload, all of the attributes of each individual resource, and the effort involved in the right-sizing operation. Right-sizing can be an iterative process, initiated by changes in usage patterns and external factors, such as AWS price drops or new AWS resource types.
- **Select the resource type, size, and number based on data**: Select the resource size or type based on data about the workload and resource characteristics: for example, compute, memory, throughput, or write intensive. This selection is typically made by using a previous (on-premises) version of the workload, using documentation, or using other sources of information about the workload. In this module, you learned how different database resources can scale and saw examples of use cases suited to different configurations. It is important to find the right balance of performance and costs to right-size your resources. For example, you saw how Aurora can provide better performance and lower cost than standard database engines and how Aurora Serverless can be used to provide on-demand scaling so that you can avoid overprovisioning resources.

## Key Takeaways: Applying AWS Well-Architected Framework Principles to Your Database Layer
- To achieve and maintain efficient workloads in the cloud, consider best practices in the performance efficiency pillar such as making selection choices based on data characteristics and access patterns.
- To secure your infrastructure effectively so that data is durable and safe from threats, consider best practices in the security pillar such as implementing secure key management and protecting data at rest.
- To meet the technical requirements of a workload with the lowest cost resource, consider best practices in the cost optimization pillar such as selecting the resource type, size, and number based on data.

---

## Module summary
This module prepared you to do the following: 
- Compare database types and services offered by Amazon Web Services (AWS).
- Explain when to use Amazon Relational Database Service (Amazon RDS).
- Describe the advanced features of Amazon RDS.
- Explain when to use Amazon DynamoDB.
- Identify AWS purpose-built database services.
- Describe how to migrate data into AWS databases.
- Design and deploy a database server.
- Use the AWS Well-Architected Framework principles when designing a database layer.
