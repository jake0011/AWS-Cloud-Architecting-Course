# Module 4: Adding a Storage Layer with Amazon S3

## Section 1: Introduction
- Focus: Using Amazon S3 as a storage layer in cloud architectures.  

## Objectives
- Define Amazon S3 and its functionality.  
- Identify problems S3 solves.  
- Describe methods for moving data in/out of S3.  
- Manage storage efficiently with S3.  
- Recommend use cases for S3.  
- Configure a static website on S3.  
- Apply AWS Well-Architected Framework to storage design.  

## Module Overview
**Sections**
- Defining Amazon S3  
- Using Amazon S3  
- Moving data to/from S3  
- Storing content with S3  
- Designing with S3  
- Applying Well-Architected Framework principles  

**Demos**
- S3 Transfer Acceleration  
- Lifecycle Management  
- Versioning  

**Activity**
- Designing with S3  

**Knowledge Checks**
- 10-question quiz  
- Sample exam question  

## Hands-on Lab
- **Challenge (Café) Lab:** Create a static website for a café.  
- Full details provided in student guide and lab environment.  

## Architect’s Perspective
As a cloud architect working with S3:  
- Consider **access patterns** and **business use cases** to balance cost, performance, and compliance.  
- Apply **security best practices** to prevent unauthorized access and accidental data loss.  
- Always design architectures starting from business needs (example: café scenario).  

---
## Section 2: Defining Amazon S3

### Types of Storage
- **Block Storage**  
  - Data split into fixed-size blocks.  
  - Managed by applications and file systems.  
  - Blocks can be stored across different systems.  

- **File Storage**  
  - Data organized hierarchically (folders/files).  
  - Similar to shared network drives.  

- **Object Storage**  
  - Data stored as objects with attributes + metadata.  
  - Each object = data, metadata, and unique key.  
  - Entire object updated when changes are made.  

---

### Amazon S3 Basics
- Stores unlimited unstructured data.  
- Data stored as **objects** inside **buckets**.  
- **Max object size:** 5 TB.  
- Objects have globally unique URLs (universal namespace).  
- Each object includes:  
  * Key (name/path)  
  * Version ID  
  * Value (content, immutable)  
  * Metadata (system + user-defined)  
  * Sub-resources (extra info)  

---

### S3 Components
- **Bucket:**  
  - Top-level container for objects.  
  - Globally unique name.  
  - Regional endpoint:  
    ```
    https://s3-<region>.amazonaws.com/<bucket-name>/<object-key>
    ```
  - Controls namespace, billing, access, reporting.  

- **Object:**  
  - Core storage unit (e.g., text, video, photo).  
  - Contains object data + metadata.  
  - Identified by a **key** (unique per bucket).  
  - Stored regionally (never leaves unless transferred).  

---

### Using Prefixes
- Prefixes simulate a folder structure.  
- Example bucket: `graphics-bucket`  
  - Objects like:  
    ```
    photos/2022/catpiano.jpg
    photos/2021/lakefront.png
    video-source/9984.mp4
    ```
- Querying with prefix `photos/2022` retrieves only 2022 photo objects.  

---

### Benefits of Amazon S3
- **Durability**  
  - 99.999999999% ("11 nines").  
  - Redundant storage across devices/facilities.  
  - Data integrity checked with checksums.  

- **Availability**  
  - 99.99% ("4 nines").  
  - Virtually unlimited capacity.  
  - Strong access control + encryption support.  

- **High Performance**  
  - Thousands of transactions/sec.  
  - Scales automatically to high request rates.  

---

### Key Takeaways
- Three storage types: block, file, object.  
- Amazon S3 = **object storage** service.  
- Stores massive unstructured data.  
- **Bucket = container**, **Object = core unit**.  
- Benefits: **durability, availability, performance**.  

---
## Section 3: Using Amazon S3

### Common Customer Use Cases
- **Handle traffic spikes** → Store web content that needs to scale quickly.  
- **Static site hosting** → Host HTML, images, videos without servers.  
- **Data for analysis** → Store data for analytics and processing.  
- **Disaster recovery & backup** → Durable, scalable storage for backups.  

---

### Use Case: Media Hosting
- Store and distribute **videos, images, music, media files**.  
- Each object has a unique HTTP URL for direct delivery.  
- Can serve as **origin store** for a CDN (e.g., CloudFront).  
- Elasticity supports sudden **demand spikes**.  
- Useful for **fast-growing, user-generated content sites**.  

---

### Use Case: Hosting Static Websites
- Host **static content** (HTML, CSS, JS, images, videos).  
- **Not for dynamic websites** (no server-side scripting).  
- Steps:  
  1. Configure S3 bucket for website hosting.  
  2. Attach bucket policy for public access.  
  3. Upload website content.  
- Benefits: **low cost, high performance, scalable, no servers needed**.  

---

### Use Case: Data Store for Analytics
- Ideal for **financial analysis, clickstream, media processing**.  
- Supports large-scale workloads via horizontal scaling.  
- Example workflow:  
  1. Spin up EC2 Spot Fleet or EMR cluster.  
  2. Extract raw data from S3 (and other sources).  
  3. Run compute algorithms to integrate & transform data.  
  4. Store processed data back in S3.  
  5. Terminate compute capacity to save costs.  
  6. Use analytics tools (e.g., QuickSight) for insights.  

---

### Use Case: Backup & Archival
- Backup from **on-premises data centers** or **EC2 instances**.  
- Benefits: **durable, scalable, cost-effective**.  
- Options:  
  - **Cross-Region Replication** → auto-copy objects to another region.  
  - **S3 Glacier** → move long-term data to archival storage.  

---

### Key Takeaways: Using Amazon S3
- Store/distribute **media content**.  
- Support **static site hosting**.  
- Serve as **data store for analytics**.  
- Provide **backup & archival solutions**.  

## Section 4: Moving Data to and from Amazon S3

### Storing Objects in S3
- Unlimited objects per bucket.  
- Upload requires **write permission**.  
- Any file type allowed (e.g., images, backups, movies, data).  
- **Default encryption** with SSE-S3 (server-side, AWS-managed keys).  
- Objects are **encrypted on upload** and **decrypted on download**.  

---

### Options for Uploading Objects
- **AWS Management Console** → wizard-based, drag & drop, max 160 GB per file.  
- **AWS CLI** → upload/download from terminal or scripts; supports multipart uploads.  
- **AWS SDKs** → programmatic uploads with wrapper libraries.  
- **Amazon S3 REST API** → send `PUT` requests for single or multipart uploads.  
- Multipart upload required for files **>160 GB** (up to **5 TB**).  

---

### Feature: Multipart Upload
- Splits a single object into **parts**.  
- Parts uploaded **independently, in parallel, in any order**.  
- **Advantages**:  
  - Higher throughput (parallel transfers).  
  - Faster recovery from network errors (retransmit only failed part).  
  - Pause & resume uploads anytime.  
  - Start upload before final object size is known.  

---

### S3 Transfer Acceleration
- **Fast, secure transfers** over long distances.  
- Uses **CloudFront edge locations** to route data to S3.  
- Improves large cross-continent transfers by **50–500%**.  
- Best for:  
  - Global customer uploads to a central bucket.  
  - Large (GB–TB) frequent transfers.  
  - Cases where internet bandwidth isn’t fully utilized.  

---

### Demo: S3 Transfer Acceleration
- Demonstrates how to **enable Transfer Acceleration** on a bucket.  
- Shows how to **access accelerated transfers**.  
- (Find demo recording in course module.)  

---

### AWS Transfer Family
- Fully managed service for **secure file transfers** into/out of:  
  - **Amazon S3**  
  - **Amazon EFS**  
- Supports protocols:  
  - **SFTP (v3)**  
  - **FTPS**  
  - **FTP**  
  - **AS2**  

#### Benefits
- Scales in real time, no infra to manage.  
- Integrates with native AWS services for **analytics, reporting, auditing, archival**.  
- **Amazon EFS** scales automatically to petabytes.  
- Offers **serverless workflows** for automated file transfers.  
- **Pay-as-you-go**, no upfront costs.  

#### Use Cases
- **Amazon S3**:  
  - Data lakes (third-party uploads).  
  - Subscription-based data distribution.  
  - Internal org transfers.  
- **Amazon EFS**:  
  - Data distribution.  
  - Supply chain management.  
  - Content management.  
  - Web-serving apps.  

---

### Key Takeaways: Data Transfer in S3
- Upload with **Console, CLI, SDKs, or REST API**.  
- Use **multipart upload** for files ≥100 MB (required for very large objects).  
- **S3 Transfer Acceleration** boosts long-distance transfer performance.  
- **AWS Transfer Family** provides secure, protocol-based transfers to S3/EFS.  

---

## Section 4: Adding a Storage Layer with Amazon S3
Amazon S3 provides scalable object storage for different use cases, including applications, websites, content distribution, and backups.

### S3 Storage Classes

### General Purpose
- **S3 Standard**: High durability (11 nines), availability, low latency, and high throughput.  
  Suitable for frequently accessed data (apps, websites, analytics, gaming, mobile).

### Intelligent Tiering
- **S3 Intelligent-Tiering**: Automatically optimizes costs by moving objects between frequent, infrequent, and archive tiers.  
  No performance impact, retrieval fees, or operational overhead. Ideal for data lakes, analytics, and user-generated content.

### Infrequent Access
- **S3 Standard-IA**: Same durability as Standard, but cheaper for infrequently accessed data.  
  30-day minimum storage and retrieval costs apply.  
- **S3 One Zone-IA**: Stores data in one Availability Zone. Lower cost but less resilient.  
  Best for secondary backups or recreatable data.

### Archive
- **S3 Glacier Instant Retrieval**: Low-cost archive for rarely accessed data needing millisecond retrieval (e.g., medical images, media archives).  
- **S3 Glacier Flexible Retrieval**: Lower cost for backups/disaster recovery; data retrieved asynchronously (minutes–hours).  
- **S3 Glacier Deep Archive**: Lowest-cost option for long-term retention (7–10+ years), compliance-heavy industries.  
- **S3 on Outposts**: Brings S3 object storage to on-premises AWS Outposts. Supports local data residency with standard S3 APIs.

---

## S3 Storage Class Comparison
- **Durability**: 99.999999999% (all classes).  
- **Availability**: 99.9% (except One Zone-IA at 99.5%).  
- **SLAs**: 99%.  
- **Minimum Storage Duration**:  
  - 30 days: Standard-IA, One Zone-IA  
  - 90 days: Glacier Instant Retrieval, Glacier Flexible Retrieval  
  - 180 days: Glacier Deep Archive  
- **Retrieval Charges**: Apply to all except Standard and Intelligent-Tiering.  
- **AZ Coverage**: ≥3 (except One Zone-IA: 1 AZ).  

---

## S3 Lifecycle Configurations
Lifecycle rules manage object transitions and expirations:
- **Transition actions**: Move objects between classes (e.g., Standard → IA after 30 days; IA → Glacier after 1 year).  
- **Expiration actions**: Delete objects when no longer needed.  
- Policies reduce costs by automatically cycling data across storage tiers.

### Examples
1. Logs kept for 1 month → auto-delete.  
2. Archival of regulatory data (e.g., finance, healthcare).  
3. Documents accessed frequently for a period → later archived.  

**Demo Slide: Lifecycle**  
- Console walkthrough showing setup of transition and expiration policies.

---

## Amazon S3 Versioning
Protects against accidental deletes/overwrites by keeping multiple versions of an object.  
- **Enabled**: New versions created on overwrite; delete adds a marker (old versions recoverable).  
- **Disabled**: Overwrites and deletes are permanent.  
- **Suspended**: Keeps old versions but behaves like disabled for new uploads.  

> Versioning cannot be disabled once enabled, only suspended.  
> Storage costs apply for each version stored.

### Versioning Actions
- **Upload in versioned bucket**: New version ID created; old retained.  
- **Delete in versioned bucket**: Delete marker added, old versions still retrievable.  
- **GET request**: Returns most recent version (404 if delete marker is current).  
- **Retrieve by version ID**: Specific version returned even if not current.  
- **Permanent delete**: Specify version ID; no delete marker added, version unrecoverable.  

**Demo Slide: Versioning**  
- Shows overwrites, delete markers, and restoring previous versions.

---

## Cross-Origin Resource Sharing (CORS)
CORS allows client apps from one domain to access S3 resources in another domain.  
- Configured with XML rules specifying origins, methods, and conditions.  
- Example: Hosting fonts in S3 accessed by websites from another domain.  

**Demo Slide: CORS**  
- Console example of configuring bucket CORS policies.

---

## S3 Data Consistency Model
- Strong consistency across all Regions.  
- **Read-after-write consistency** for GET, LIST, PUT, DELETE.  
- Simplifies big data workloads and migration of analytics workloads.

---

## Key Takeaways
Amazon S3 is often used to:  
- Store and distribute videos, photos, music files, and other media.  
- Host static content (HTML files, images, videos).  
- Serve as a data store for computation and analytics.  
- Provide backup and archival solutions.

---
# Designing with Amazon S3

## Adding a Storage Layer with Amazon S3
Amazon S3 is widely used as a storage layer within AWS architectures. This section focuses on how to design with S3 effectively, ensuring security, encryption, access management, cost efficiency, and compliance.

---

## 1. Default Security Configurations in Amazon S3
- **Private by default**: All new S3 buckets and objects are created private and protected. Only explicitly authorized users or accounts can access them.
- **Encryption by default**:  
  - Buckets automatically apply **server-side encryption with Amazon S3 managed keys (SSE-S3)**.  
  - This ensures data is encrypted at rest without requiring user intervention.
- **Principle of least privilege**:  
  - When sharing data, carefully manage and control access.  
  - Grant only the permissions absolutely necessary.  

**Why encryption matters:**  
Encryption takes legible data and encodes it using a secret key. Without that key, the data is unreadable—even if attackers gain access. Optionally, AWS Key Management Service (AWS KMS) can be used for secure key management.

---

## 2. Encrypting Objects in Amazon S3
Encryption can occur **server-side** or **client-side**:

- **Server-Side Encryption (SSE)**  
  - S3 encrypts objects before saving them to disk and decrypts them when retrieved.  
  - By default, all new objects are encrypted using **SSE-S3**.  
  - Other server-side options include:  
    - **SSE-KMS** (encryption with AWS KMS keys)  
    - **DSSE-KMS** (dual-layer KMS encryption)  
    - **SSE-C** (customer-provided keys)

- **Client-Side Encryption**  
  - Data is encrypted **before** it’s uploaded to S3.  
  - The user manages the encryption keys and process.  
  - This approach adds another security layer since data is already encrypted before AWS handles it.

---

## 3. Tools for Protecting Buckets and Objects
AWS provides multiple tools to control access:

- **Block Public Access**  
  - Overrides other policies to prevent public access.  
  - Strongly recommended for buckets that should remain private.

- **IAM Policies**  
  - Identity-based policies that authenticate users and roles.  

- **Bucket Policies**  
  - Define rules directly on a bucket.  
  - Can grant access across accounts or even public access (though risky).  
  - Deny statements take precedence over allow permissions.  

- **Access Control Lists (ACLs)**  
  - An older method predating IAM. Less preferred, but still available.  

- **Amazon S3 Access Points**  
  - Provide unique access endpoints for specific applications.  
  - Useful for organizations with multiple datasets and varied access needs.

- **Pre-signed URLs**  
  - Grant temporary, time-limited access to S3 objects.  

- **AWS Trusted Advisor**  
  - Offers security checks, including detection of buckets with global access permissions.

---

## 4. General Approaches to Access Configuration
1. **Default (Private Access)**  
   - Buckets are private by default.  
   - Only root users or admins have access until permissions are granted.

2. **Controlled Access**  
   - Specific users or groups are allowed or denied access.  
   - Achieved through IAM, bucket policies, or access points.  

3. **Public Access (Not Recommended)**  
   - Anyone on the internet can access the objects.  
   - Rarely appropriate—only valid for cases like hosting static websites.  
   - Most use cases (e.g., backups, application data) should avoid public access entirely.

---

## 5. Considerations When Choosing a Region
Several factors influence where to host S3 data:

- **Data Privacy & Compliance**  
  - Local laws may restrict where data can be stored.  
  - Some regulations (e.g., HIPAA) impose strict requirements.  

- **Proximity to Users**  
  - Lower latency improves customer experience.  
  - Even small latency differences can matter in sensitive applications.  

- **Service Availability**  
  - Not all AWS services are available in every Region.  
  - Services expand gradually, so availability varies.  

- **Cost-Effectiveness**  
  - Pricing varies across Regions.  
  - While costs may be secondary to compliance or latency, they can influence Region choice when differences are minimal.  

- **Multi-Region Deployment**  
  - For global customers, replicating environments across Regions can reduce latency.  
  - This approach improves performance but adds management complexity and potential costs.

---

## 6. Amazon S3 Inventory
- Provides **auditing and reporting** tools for compliance and operational efficiency.  
- Can generate daily or weekly reports in **CSV, ORC, or Parquet** formats.  
- Reports include object metadata (e.g., replication status, encryption status).  
- Helps speed up **big data workflows** by replacing synchronous `LIST` API operations with scheduled exports.  
- **Source bucket**: The bucket being reported on.  
- **Destination bucket**: The bucket where inventory reports are stored.  
- Reports can be queried using **Athena, Redshift Spectrum, Presto, Hive, or Spark**.

---

## 7. Amazon S3 Costs
- **Pay-as-you-go model** (no minimum fee).  
- **Charges apply for**:  
  - Storage (per GB, per month, per storage class).  
  - PUT, COPY, POST, LIST requests, and lifecycle transitions.  
  - Monitoring in S3 Intelligent-Tiering.  

- **No charge for data transfer**:  
  - First 100 GB per month out to the internet.  
  - Inbound data transfers.  
  - Transfers between S3 buckets or within the same Region.  
  - Data sent to CloudFront.  

- **Encryption costs**:  
  - SSE-S3: No extra charge.  
  - SSE-C: No extra charge.  
  - SSE-KMS: Additional KMS charges for key use.  
  - DSSE-KMS: Adds a per-GB encryption/decryption fee.

- **AWS Free Tier**:  
  - 5 GB storage in S3 Standard  
  - 20,000 GET requests  
  - 2,000 PUT/COPY/POST/LIST requests  
  - 100 GB outbound transfer per month  

---

## 8. Activity: Designing with Amazon S3
**Scenario:** A café owner hosts a static website but now wants private employee-only access to documents like cooking instructions, vendor info, payroll, and compliance training.  

**Solution:**  
- Organize content into folders (e.g., cooking, vendors, payroll, compliance).  
- Set up **IAM policies** (individual or group) to control access based on employee roles.  
- Avoid public access to ensure confidentiality.  

---

## 9. Key Takeaways
- S3 buckets are **private by default** and require explicit access permissions.  
- **SSE-S3** is the default encryption mechanism for data at rest.  
- **Region selection** involves trade-offs in compliance, latency, availability, and cost.  
- **Costs** are based on storage, requests, and encryption type, but you pay only for what you use.  
- Tools like **S3 Inventory, IAM policies, access points, and pre-signed URLs** enhance security and management.  

---

## Debrief: Café Website Lab
- **Accidental overwrite/deletion protection:** Used **versioning** to maintain prior object versions and recover from accidental changes.  
- **Cost-saving strategy:** Leveraged **S3 Versioning with lifecycle policies** to automatically transition older versions to more cost-effective storage classes.  

---

## AWS Well-Architected Framework Overview
The AWS Well-Architected Framework is built around six pillars, each with best practices and guiding questions for designing resilient cloud workloads.  
Relevant pillars for this module:  
- **Security**  
- **Reliability**  
- **Performance Efficiency**  
- **Cost Optimization**

---

## Best Practice: Data Protection (Security Pillar)
- **Enforce Encryption at Rest**  
  - Protects data confidentiality if unauthorized access occurs.  
  - Encryption ensures sensitive data cannot be read without decryption keys.  
  - Unencrypted data should be inventoried and controlled.  

- **Enforce Access Control**  
  - Apply the **principle of least privilege** (only authorized users on a need-to-know basis).  
  - Use mechanisms such as **IAM policies, bucket policies, isolation, and versioning**.  
  - Regular backups and versioning defend against intentional or accidental data loss.  
  - Prevent public access unless absolutely necessary.  

**Supporting S3 Features:**  
- Buckets and objects are private and encrypted by default.  
- IAM and bucket policies can strictly limit access.  
- **Versioning** protects against accidental deletions or overwrites.  
- **Block Public Access** prevents unintended exposure.  

---

## Best Practice: Architecture Selection (Performance Efficiency Pillar)
- **Understand available AWS services and features** before designing solutions.  
- **Factor cost** into decisions, balancing performance with efficiency.  
- **S3 as object storage:** Suitable for storing massive unstructured datasets.  
- **Performance-enhancing options:**  
  - **S3 Transfer Acceleration** for faster long-distance uploads.  
  - **Multipart upload** for large object transfers.  
- **Storage Classes:**  
  - Match data access patterns with the right class.  
  - **S3 Intelligent-Tiering** automatically optimizes for cost and performance.  

---

## Best Practice: Cost-Effective Resources (Cost Optimization Pillar)
- **Perform cost analysis over time:** Workloads evolve, so costs should be revisited regularly.  
- **S3 Lifecycle Policies:** Automatically transition data to cheaper storage classes.  
- **S3 Intelligent-Tiering:** Moves data dynamically based on access frequency.  
- **S3 Inventory:** Audits storage usage and helps identify opportunities to reduce cost.  
- Supports both **performance** and **cost optimization** pillars.  

---

## Best Practice: Failure Management (Reliability Pillar)
- **Failures are inevitable** — design for resilience.  
- **Multi-AZ deployment:** Use multiple Availability Zones for higher availability.  
- **S3 Reliability Features:**  
  - 99.999999999% (11 nines) durability.  
  - 99.99% availability.  
  - Redundant storage across multiple AZs in a Region.  
  - Automatic integrity verification with checksums.  

---

## Key Takeaways
- **Security:**  
  - Protect data with encryption, IAM/bucket policies, versioning, and Block Public Access.  
- **Performance Efficiency:**  
  - Use S3’s scalability, Transfer Acceleration, and multipart upload for optimal performance.  
- **Cost Optimization:**  
  - Employ lifecycle policies, Intelligent-Tiering, and Inventory to control costs.  
- **Reliability:**  
  - Trust S3’s high durability and availability for backups and disaster recovery.  

Amazon S3 combines **security, performance, cost efficiency, and reliability** to align with AWS Well-Architected Framework principles, making it a robust choice for cloud storage.  

---
## Module Summary

This module prepared you to do the following:

- Define Amazon S3 and how it works.  
- Recognize the problems that Amazon S3 can solve.  
- Describe how to move data to and from Amazon S3.  
- Manage the storage of content efficiently by using Amazon S3.  
- Recommend the appropriate use of Amazon S3 based on requirements.  
- Configure a static website on Amazon S3.  
- Use the Well-Architected Framework principles when designing a storage layer with Amazon S3.
