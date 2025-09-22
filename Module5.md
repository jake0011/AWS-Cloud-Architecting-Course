# Module 5: Adding a Compute Layer Using Amazon EC2

---

## Introduction
This module introduces you to using and optimizing Amazon Elastic Cloud Compute (Amazon EC2) for your computing workloads.

## Module Objectives
This module prepares you to do the following:

- Identify how to use Amazon Elastic Compute Cloud (Amazon EC2) in an architecture.  
- Explain the value of using Amazon Machine Images (AMIs) to accelerate the creation and repeatability of infrastructure.  
- Recommend EC2 instance types based on requirements.  
- Recommend storage solutions for Amazon EC2.  
- Recognize how to configure Amazon EC2 instances with user data.  
- Describe EC2 pricing options and make recommendations based on cost.  
- Launch an Amazon EC2 instance.  
- Use the AWS Well-Architected Framework principles when designing a compute layer with Amazon EC2.  


### Module Overview

### Presentation Sections
- Adding compute with Amazon EC2  
- Choosing an AMI to launch an EC2 instance  
- Selecting an EC2 instance type  
- Adding storage to an Amazon EC2 instance  
- Other EC2 configuration considerations  
- Amazon EC2 pricing options  
- Applying the AWS Well-Architected Framework principles to compute  

### Demos
- Configuring an EC2 Instance with User Data  
- Reviewing the Spot Instance History Page  

### Activity
- Choosing Instance Types  

### Knowledge Checks
- 10-question knowledge check  
- Sample exam question  

The objectives of this module are presented across multiple sections.  
You will also participate in an activity to choose instance types based on workload requirements.  

You will view two recorded demonstrations introduced in the student guide.  
The module wraps up with:  
- A 10-question knowledge check delivered in the online course  
- A sample exam question for class discussion  


## Hands-On Labs in This Module
- **Guided lab:** Introducing Amazon EFS  
- **Challenge (Café) lab:** Creating a Dynamic Website for the Café  

This module includes both a guided lab with step-by-step instructions and a café challenge lab where you update the café’s architecture.  
Additional information is provided in the student guide and detailed instructions are available in the lab environment.  


### Cloud Architect Perspective
As a cloud architect designing a compute layer using EC2:

- You need to analyze key workload characteristics to choose the AMI, EC2 instance type, and storage options that optimize performance and security.  
- You need to select an EC2 purchasing model that matches the compute use case to optimize costs.  

Keep these considerations in mind when approaching cloud network design. Always work backwards from the business need to design the best architecture for a specific use case.  

Throughout this module, consider the café scenario as an example business need and think about how you would design solutions for the fictional café business.  


## Section 2: Adding Compute with Amazon EC2
This section introduces the main compute options in the AWS Cloud, with a focus on Amazon Elastic Compute Cloud (Amazon EC2).

## AWS Runtime Compute Choices
AWS provides multiple compute services tailored for different use cases:

- **Virtual Machines (VMs)**  
  *Amazon EC2*: Secure, resizable virtual servers for workloads of all sizes.  

- **Containers**  
  *Amazon ECS*: Run Docker container applications.  
  *Amazon EKS*: Managed Kubernetes service, for highly available and scalable container apps.  

- **Virtual Private Servers (VPS)**  
  *Amazon Lightsail*: Simplified compute, storage, networking, and management tools for websites, web apps, blogs, and e-commerce, with predictable pricing.  

- **Platform as a Service (PaaS)**  
  *AWS Elastic Beanstalk*: Run applications in languages like Java, .NET, Node.js, Python, Ruby, Go, and Docker with automated deployment.  

- **Serverless**  
  *AWS Lambda*: Run code (Java, Go, PowerShell, Node.js, Python, Ruby, etc.) without provisioning servers.  
  *AWS Fargate*: Serverless compute engine for containers.  

This module focuses specifically on **Amazon EC2**.


### Compute Service Category Differentiators
Each compute category differs in management responsibilities, scalability, and deployment speed:

- **VMs, Containers, VPS**:  
  Provide more infrastructure control and customization.  

- **PaaS and Serverless**:  
  Reduce infrastructure management, enabling faster application deployment.  



### Amazon EC2
Amazon EC2 provides virtual machines in the cloud with the following features:

- Provision servers in minutes.  
- Scale capacity up or down automatically.  
- Pay only for what you use.  
- Supports multiple operating systems, including Windows, Linux, and macOS.  
- Offers flexibility in processor, storage, networking, operating system, and purchase model.  



### Amazon EC2 Virtualization
Key points on how EC2 instances work:

- An **EC2 instance** is a VM running on a physical host in an AWS Availability Zone.  
- Each VM runs its own **operating system** and applications.  
- EC2 runs on a **hypervisor** layer, maintained by AWS, which manages access to physical hardware resources.  
- **Storage options**:  
  - *Instance store*: Ephemeral storage, tied to the host.  
  - *Amazon EBS*: Persistent block storage volumes.  
- **Networking**:  
  Instances can connect to other EC2 instances, AWS services, or the internet, with configurable access for security.  
- Different instance types provide varying levels of CPU, memory, storage, and network performance.



## Amazon EC2 Use Cases
Common scenarios for using EC2 include:

- When you need **full control** of computing resources, including OS, processor, and accelerators.  
- When optimizing **compute costs** with options like On-Demand, Reserved, Spot Instances, or Savings Plans.  
- To run a **wide range of workloads**, from simple websites to enterprise apps to generative AI.  

---

### Steps for Provisioning an EC2 Instance
Provisioning involves several key steps:

1. **Select an AMI** (Amazon Machine Image) – choose from AWS, third parties, or custom-created images.  
2. **Choose an instance type** – based on CPU, memory, storage, and networking needs.  
3. **Set up a key pair** – essential for secure SSH/RDP access.  
4. **Configure networking** – placement within a VPC, assign IP or DNS as needed.  
5. **Assign a security group** – define firewall rules to control inbound/outbound traffic.  
6. **Select storage** – boot from instance store or EBS, and attach additional volumes as needed.  
7. **Attach an IAM role** – use instance profiles to securely grant permissions for AWS services.  
8. **Add user data (optional)** – automate setup tasks or software installation at launch.  

Security is critical: configure key pairs, security groups, and ensure updates/patches are applied.

---

### Key Takeaways – Adding Compute with EC2
- EC2 enables scalable, flexible virtual machines in the cloud.  
- Best suited when you need **full control** over compute resources.  
- Launching an instance involves choosing an AMI and instance type, and configuring network, security, storage, and user data.  


---
# Section: Adding a Compute Layer Using Amazon EC2

### Amazon Machine Image (AMI)
- An AMI contains the necessary information to launch an EC2 instance.  
- Components include:
  * **Template for the root volume** – contains the OS and installed software.  
  * **Launch permissions** – control who can access the AMI and whether it is public.  
  * **Block device mappings** – specify additional storage volumes to attach.  
- You must specify a source AMI when launching an instance.  
- Multiple instances can be launched from the same AMI, or different AMIs can be used for various roles (e.g., web server vs. application server).  

### Benefits of AMIs
- **Repeatability** – launch multiple identical instances with consistency.  
- **Reusability** – all instances from the same AMI are exact replicas.  
- **Recoverability** – create an AMI from a configured instance for backup. Failed instances can be replaced by launching from the same AMI.  
- Best practice: save new software or configuration changes by creating a new AMI to preserve the latest setup.  

### Choosing an AMI
Key considerations:
- **Region** – AMIs exist in a specific AWS Region; copy across Regions if needed.  
- **Operating system** – Windows or Linux variants (Amazon Linux, Ubuntu, Red Hat, etc.).  
- **Root device storage** – either EBS-backed or instance store-backed.  
- **Architecture** – choose based on processor type (32-bit/64-bit, x86/ARM).  
- **Virtualization type** – Paravirtual (PV) or Hardware Virtual Machine (HVM). For best performance, use **HVM**.  

AMI sources:
- **Quick Start** – AWS-provided Windows and Linux AMIs.  
- **My AMIs** – custom AMIs created from EC2 instances.  
- **AWS Marketplace** – preconfigured templates from third-party vendors.  
- **Community AMIs** – shared publicly by others (use with caution, not recommended for production).  

### Instance Store-Backed vs Amazon EBS-Backed AMIs
- **Boot time** – EBS-backed boots faster; instance store-backed takes longer.  
- **Root device size** – up to 16 TiB (EBS) vs. 10 GiB (instance store).  
- **Stopping instances** – EBS-backed instances can be stopped; instance store-backed cannot.  
- **Changing instance type** – possible with EBS (stop → change → restart); not possible with instance store.  
- **Costs** –  
  * EBS-backed: pay for instance usage, EBS volumes, and EBS snapshot storage.  
  * Instance store-backed: pay for instance usage and AMI storage in S3 (usually cheaper).  
- **Use cases** –  
  * EBS-backed: when persistent storage is required (databases, web apps).  
  * Instance store-backed: for temporary storage that doesn’t need persistence.  

### Amazon EC2 Instance Lifecycle
- **Pending** → **Running** → (actions: reboot, stop, terminate).  
- **Rebooting** – instance stays on same host with same DNS and IP.  
- **Stopping (EBS only)** – moves to `stopping` → `stopped`; later can be restarted.  
- **Terminating** – moves to `shutting-down` → `terminated`; cannot be recovered.  
- **Hibernate (EBS only)** – saves in-memory data, IPs, and Elastic IPs for resuming later.  

### Creating a New AMI
- Start from a **source AMI** (AWS Quick Start, custom, or imported VM).  
- Launch an EC2 instance → configure it into a **golden instance** → capture it as a new AMI.  
- EBS-backed AMIs: create an image, AWS registers it automatically.  
- Instance store-backed AMIs: bundle root volume, upload to S3, register manually.  
- Newly created AMIs can be copied across Regions.  

### EC2 Image Builder
- Automates creation, management, validation, and deployment of AMIs.  
- Features:  
  * Graphical interface for image pipelines.  
  * Produces validated and secure images.  
  * Supports both EC2 AMIs and on-premises VM images.  
  * Provides version control for tracking changes.  
- Helps minimize vulnerabilities by allowing only essential components.  

### Key Takeaways
- AMIs define the configuration needed to launch EC2 instances.  
- Benefits include **repeatability, reusability, and recoverability**.  
- For best performance, use **HVM** virtualization.  
- AMIs can be obtained from AWS (Quick Start, Marketplace), created by you (My AMIs), or shared by others (Community).  

---
## Section: Selecting an Amazon EC2 Instance Type

### EC2 Instance Type Configuration
- An instance type defines the **CPU, memory, storage, and network performance**.  
- Example configurations:  

| Instance Type  | vCPU | Memory  | Storage         | Network Performance     |
|----------------|------|---------|-----------------|-------------------------|
| m5d.large      | 2    | 4 GiB   | 1 × 50 NVMe SSD | Up to 10 Gbps           |
| m5d.xlarge     | 4    | 8 GiB   | 1 × 100 NVMe SSD| Up to 10 Gbps           |
| m5d.8xlarge    | 32   | 128 GiB | 2 × 600 NVMe SSD| 10 Gbps (consistent)    |

- As instance size increases → **more vCPUs, memory, and storage**.  
- Network performance:
  * m5d.large & m5d.xlarge: up to 10 Gbps.  
  * m5d.8xlarge: consistent 10 Gbps.  
- Choosing the right type requires considering **workload performance needs** and **cost requirements**.  
- All current generation instances support **enhanced networking**, except **T2 instances**.  
- Enhanced networking helps workloads like SIP (Session Initiation Protocol) by ensuring consistent bandwidth and lower latency.  

---

### EC2 Instance Type Naming Convention
- Components of a type name:  
  * **Family**  
  * **Generation**  
  * **Processor family** (optional)  
  * **Additional capabilities** (optional)  
  * **Size**  
- Example: `c7gn.xlarge`  
  * `c` → compute family  
  * `7` → 7th generation  
  * `g` → Graviton processor  
  * `n` → network + EBS optimized  
  * `xlarge` → size  
- Higher generation numbers = generally more powerful & better value.  
- Some types do not include processor family/capability letters (e.g., `m5d.xlarge`).  

---

### Suitability of Instance Types for Workloads

**General Purpose**  
- Balanced compute, memory, networking.  
- Workloads: web/app servers, enterprise apps, gaming, dev/test environments.  
- Examples: M7, Mac, M6, M5, M4, T4, T3, T2.  

**Compute Optimized**  
- Suited for compute-bound workloads.  
- Workloads: batch processing, distributed analytics, HPC, ad engines, multiplayer gaming, video encoding.  
- Examples: C7, C6, C5, C4.  

**Storage Optimized**  
- High sequential read/write, large datasets.  
- Workloads: high-performance databases, NoSQL, real-time analytics, transactional workloads, big data, log processing.  
- Examples: I4, Im4, Is4, I3, D2, D3, H1.  

**Memory Optimized**  
- Large datasets in memory.  
- Workloads: in-memory caches, high-performance databases, big data analytics.  
- Examples: R7, R6, R5, R4, X2, X1, Z1.  

**Accelerated Computing**  
- Hardware accelerators (GPUs, FPGAs, etc.).  
- Workloads: ML/AI, HPC, graphics workloads.  
- Examples: P5, P4, P3, P2, DL1, Trn1, Inf2, Inf1, G5, G4, G3, F1, VT1.  

**HPC Optimized**  
- Best price/performance for HPC at scale.  
- Workloads: simulations, deep learning, compute-intensive HPC.  
- Examples: Hpc7, Hpc6.  

---

### Choosing an Instance Type
- Over 270 instance types exist.  
- Selection depends on **performance requirements** and **cost efficiency**.  
- Tips:  
  * New instances → use the **Instance Types page** in EC2 console to filter/search.  
  * Existing instances → use **AWS Compute Optimizer** for recommendations.  
  * Best practice: choose **latest generation** in a family (better price/performance).  
  * Slightly under-spec at first → scale/resize later if needed.  
  * Oversizing initially is wasteful and may hide performance issues.  

---

### AWS Compute Optimizer
- Analyzes configuration & utilization metrics of EC2 + Auto Scaling groups.  
- Provides recommendations on:  
  * **Instance type**  
  * **Instance size**  
  * **Auto Scaling group configuration**  
- Uses **Amazon Machine Learning** to generate insights.  
- Supports: M, C, R, T, X families.  
- Findings categories:  
  * **Under-provisioned**  
  * **Over-provisioned**  
  * **Optimized**  
  * **None** (not enough data or unsupported type).  

---

### Activity
- Match use cases to instance types.  
- Use: [AWS Instance Types Page](https://aws.amazon.com/ec2/instance-types/) → *Instance Type Details* section.  

---

### Key Takeaways: Selecting an EC2 Instance Type
- Instance type = CPU + memory + storage + network performance.  
- New generation types = better **price/performance**.  
- Use **EC2 console (Instance Types page)** and **AWS Compute Optimizer** to select the best fit for workloads.  

---

## Section: Adding storage to an Amazon EC2 instance

### Storage overview

| AWS EC2 storage resource | Root volume | Data volumes for a single instance | Data volumes accessible from multiple **Linux** instances | Data volumes accessible from multiple **Windows** instances |
|--------------------------|-------------|------------------------------------|----------------------------------------------------------|------------------------------------------------------------|
| Amazon EBS (SSD-backed only) | Yes | Yes | No | No |
| Instance store           | Yes | Yes | No | No |
| Amazon Elastic File System (Amazon EFS) [Linux] | No | No | Yes | No |
| Amazon FSx for Windows File Server | No | No | No | Yes |

An EC2 instance always has a root volume and can optionally have one or more data volumes. The four main storage options are **Instance store**, **Amazon EBS**, **Amazon EFS**, and **Amazon FSx for Windows File Server**. Only an instance store or an SSD-backed EBS volume can be a root volume. Use instance store for fast, non-persistent storage; use EBS for persistent root or data volumes. To share storage across instances, use EFS (Linux) or FSx (Windows).

---

### Instance store
- **Characteristics**
  - Temporary block-level storage on the host computer.
  - Uses HDD or SSD (NVMe offers higher I/O).
  - Data is lost when the instance is stopped or terminated.
- **Use cases**
  - Buffers, cache, scratch data.
- Instance store volumes persist across reboots but not across stop/terminate. Instance store-backed instances cannot be stopped — only rebooted or terminated.

---

### Amazon EBS
- **Characteristics**
  - Persistent, network-attached block storage.
  - Can attach to any instance in the same Availability Zone.
  - Uses HDD or SSD; can be encrypted.
  - Supports snapshots persisted to Amazon S3.
  - Data persists independently of the instance lifecycle.
- **Use cases**
  - Stand-alone databases, general application data storage.
- EBS volumes provide low access latency suitable for databases and boot volumes.

---

### Amazon EBS — SSD-backed volume types
- **General Purpose SSD (gp2)**
  - Balances price and performance for a wide variety of workloads.
  - Recommended for most workloads; can be a boot volume.
- **Provisioned IOPS SSD (io1)**
  - Highest-performance SSD volume.
  - Suited for mission-critical, low-latency, or high-throughput workloads (large DBs, transactional workloads).
  - Can be a boot volume.
- SSD-backed volumes are optimized for IOPS-intensive transactional workloads.

---

### Amazon EBS — HDD-backed volume types
- **Throughput Optimized HDD (st1)**
  - Low-cost, designed for frequently accessed, throughput-intensive workloads (streaming, big data, data warehouses, log processing).
  - Cannot be a boot volume.
- **Cold HDD (sc1)**
  - Lowest-cost HDD for less frequently accessed, throughput-oriented storage (large cold datasets).
  - Cannot be a boot volume.

---

### Amazon EBS–optimized instances
- **Benefits**
  - Provide a dedicated network connection to attached EBS volumes.
  - Increase I/O performance and provide performance consistency.
  - Nitro-based instance types offer additional performance improvements.
- **Usage**
  - For EBS-optimized instance types, optimization is enabled by default.
  - For other instance types that support it, enable EBS optimization at launch.

---

### Shared file systems for EC2 instances
- **EBS / Instance store** → attach to a single instance only.  
- **Amazon S3** → possible but not ideal for shared, file-system-like access (object store semantics overwrite whole files).  
- **Best options for shared file systems**
  - **Amazon EFS** — shared file system for Linux instances.
  - **Amazon FSx for Windows File Server** — shared file system for Windows instances.

EFS and FSx provide the performance and read/write semantics of a network file system required when multiple instances must access the same files concurrently.

---

### Amazon Elastic File System (Amazon EFS)
- **Characteristics**
  - File system storage for Linux-based workloads.
  - Fully managed, elastic file system that scales automatically from gigabytes to petabytes.
  - Supports NFS (Network File System) protocols and strong consistency/file locking.
  - Mounts to EC2 instances; compatible with Linux-based AMIs.
- **Use cases**
  - Home directories, enterprise app file systems, app testing & development, database backups, web serving/content management, media workflows, big data analytics.
- **Example mount**

```bash
  $ sudo mount -t nfs4 mount-target-DNS:/ ~/efs-mount-point
````

---

### Amazon EFS storage classes & mount targets

* **Standard class**

  * Create a mount target in each Availability Zone for the file system.
  * Use Standard for actively accessed data needing highest durability/availability.
* **One Zone class**

  * Single mount target in the same AZ as the file system.
  * Cheaper (example: \~47% lower price) for workloads that do not require multi-AZ resilience.
* Recommendation: access the file system from a mount target in the same Availability Zone for performance and cost reasons.

---

### Amazon FSx for Windows File Server

* **Characteristics**

  * Fully managed shared file system for Windows EC2 instances.
  * Native Windows compatibility (NTFS), SMB protocol (2.0–3.1.1), DFS namespaces/replication.
  * Integrates with Microsoft Active Directory and supports Windows ACLs.
  * Backed by high-performance SSD storage.
* **Use cases**

  * Home directories, lift-and-shift Windows apps, media & entertainment workflows, data analytics, web serving & content management, software development environments.

---

### Key takeaways

* Storage options for EC2 instances: **Instance store**, **Amazon EBS**, **Amazon EFS**, **Amazon FSx for Windows File Server**.
* For a **root volume**, use **instance store** or **SSD-backed Amazon EBS**.
* For a **data volume serving a single instance**, use **instance store** or **Amazon EBS**.
* For a **data volume serving multiple Linux instances**, use **Amazon EFS**.
* For a **data volume serving multiple Windows instances**, use **Amazon FSx for Windows File Server**.

---

## Section: Other EC2 configuration considerations

### Adding a Compute Layer Using Amazon EC2
- Covers:
  * User data for initialization scripts.
  * AMI deployment strategies.
  * Placement groups (high-level).

---

### EC2 instance user data
- **Definition**
  - User data = script (shell / cloud-init) passed to instance OS at launch.
  - Runs as root/Admin after boot but before network access.
- **Uses**
  * Patch/update software.
  * Fetch license keys.
  * Install extra software.
- **Example tasks**
  * Update all packages.
  * Start Apache HTTP server.
  * Enable auto-start of Apache on boot.
- **cloud-init**
  - Open-source bootstrap tool for Linux (Amazon Linux, Ubuntu, etc.).
  - Automatically configures `.ssh/authorized_keys` for login.
  - Accepts custom user directives in user data.
  - Logs:
    - Linux: `/var/log/cloud-init-output.log`
    - Windows: `C:\ProgramData\Amazon\EC2-Windows\Launch\Log\UserdataExecution.log`
  - On Windows, processed by EC2Config or EC2Launch.

---

### Instance metadata
- **Definition**
  - Provides runtime instance info (only accessible inside instance).
- **Access**
  - URL: `http://169.254.169.254/latest/meta-data/`
  - Examples:
    | Metadata         | Value                                                   |
    |------------------|---------------------------------------------------------|
    | instance-id      | i-1234567890abcdef0                                     |
    | mac              | 00-1B-63-84-45-E6                                       |
    | public-hostname  | ec2-203-0-113-25.compute-1.amazonaws.com                |
    | public-ipv4      | 67.202.51.223                                           |
    | local-ipv4       | 10.251.50.12                                            |
  - User data retrieval: `http://169.254.169.254/latest/user-data`
- **Use cases**
  * Scripts query metadata for IPs, hostnames, MACs.
  * Mirrors AWS Console info.

---

### Working with user data on running instances
- Runs only at **first launch** by default.  
- To re-run or modify:
  1. Stop instance.
  2. Edit user data script.
  3. Remove:
     ```bash
     sudo rm /var/lib/cloud/instances/*/sem/config_scripts_user
     ```
  4. Re-run:
     * Restart instance → runs at boot.  
     * Or run manually:
       ```bash
       /var/lib/cloud/instance/scripts/part-001
       ```
⚠️ If step 3 is skipped, changes won’t run.

---

### Best practices for manual commands
- Always add manual commands back into user data script.
- Acts as a record + reusable setup.
- *Example*:
  - Server A: updated script → reusable.  
  - Server B: only manual changes → script outdated.

---

### AMI deployment models
| Model       | Description                     | Pros                            | Cons                           |
|-------------|---------------------------------|---------------------------------|--------------------------------|
| **Basic**   | OS-only, all configs at launch  | Short build time, flexible      | Slower boot                    |
| **Silver**  | Partial configs (mutable)       | Balance build/boot, flexible    | Some manual/user data required |
| **Golden**  | Fully customized (immutable)    | Fast boot, consistent behavior  | Longer build, short lifespan   |

---

### Placement groups
- **Purpose**: Control instance placement on AWS hardware.
- **Benefits**
  * Better network performance.
  * Reduce correlated failures.
- **Limits**
  * One placement group per instance.
  * Host tenancy excluded.
- **Types**
  * **Cluster** → pack close, low latency, high throughput.
  * **Partition** → spread across partitions, fault isolation.
  * **Spread** → distribute small # instances, minimize correlated failure.

---

### Key takeaways
- User data = init scripts (shell/cloud-init).
- Instance metadata = runtime instance info.
- AMI models: Basic, Silver, Golden.
- Placement groups = performance + resiliency control.

---

## Section: Amazon EC2 Pricing Options  
*Adding a Compute Layer Using Amazon EC2*  

This section explains Amazon EC2 pricing and how to choose the best payment method depending on the instance use case.  

---

### AWS Free Tier: Amazon EC2  
- **12 months free:**  
  - 750 hours per month of `t4g.small` (region-dependent)  
  - 750 hours per month of Linux, RHEL, or SLES `t2.micro` or `t3.micro` (region-dependent)  
  - 750 hours per month of Windows `t2.micro` or `t3.micro` (region-dependent)  
- **Types of free offers:**  
  - *12-Months Free*: For new AWS customers. Expires after 12 months or when usage exceeds limits.  
  - *Always Free*: Available indefinitely to both new and existing AWS customers.  
  - *Trials*: Short-term offers that start at first use, then revert to pay-as-you-go rates.  

---

### Amazon EC2 Pricing Models  
EC2 provides different strategies to optimize costs:  
- **Purchasing models** – Offer savings through various use cases.  
- **Capacity reserved models** – Guarantee reserved instances when needed.  
- **Dedicated models** – Provide dedicated hardware for compliance and regulatory needs.  

---

### Amazon EC2 Purchase Models  
- **On-Demand:**  
  - Pay per second or per hour, no long-term commitments.  
  - Best for spiky or experimental workloads.  
- **Reserved Instances:**  
  - 1-year or 3-year commitment with significant discounts.  
  - Best for predictable, steady-state workloads.  
- **Savings Plans:**  
  - Similar discounts to Reserved Instances, but more flexible.  
  - Best for all workloads needing flexibility.  
- **Spot Instances:**  
  - Bid on unused capacity at large discounts.  
  - Best for fault-tolerant, flexible, stateless workloads.  
  - Can be interrupted with a 2-minute warning.  

---

### Demo: Reviewing Spot Instance History  
A demonstration is available in the course showing how to:  
- Review Pricing History  
- Customize Pricing History view  

---

### Amazon EC2 Capacity Reservations  
- **On-Demand Capacity Reservations:**  
  - Reserve compute capacity in a specific Availability Zone.  
  - Best for high availability, regulatory requirements, or disaster recovery.  
- **EC2 Capacity Blocks for ML:**  
  - Reserve GPU instances for future ML workloads.  
  - Best for model training, experiments, or handling ML demand surges.  

---

### Amazon EC2 Dedicated Options  
Provide single-tenant hardware for compliance and licensing needs:  
- **Dedicated Instances:**  
  - Billed per instance.  
  - Hardware isolated at account level.  
- **Dedicated Hosts:**  
  - Billed per host.  
  - Provide visibility into sockets, cores, host IDs.  
  - Allow reuse of server-bound licenses and compliance control.  

---

### Amazon EC2 Cost Optimization Guidelines  
To reduce EC2 costs, combine purchase options:  
- Use **Reserved Instances** or **Savings Plans** for steady workloads.  
- Use **On-Demand Instances** for spiky or unpredictable workloads.  
- Use **Spot Instances** for fault-tolerant and flexible workloads.  

---

### Key Takeaways  
- Pricing models: On-Demand, Reserved, Savings Plans, Spot, Dedicated Hosts.  
- Per-second billing available for On-Demand, Reserved, and Spot Instances running Linux or Ubuntu.  
- Best practice: combine pricing models to achieve optimal cost savings.  


---

## Applying the AWS Well-Architected Framework principles to compute
**Adding a Compute Layer Using Amazon EC2**  
This section looks at how to apply the AWS Well-Architected Framework principles to compute resources.  
*AWS Training and Certification Module 5: Adding a Compute Layer Using Amazon EC2 © 2024, Amazon Web Services, Inc. or its affiliates. All rights reserved.*  

---

### AWS Well-Architected pillars  
The AWS Well-Architected Framework has six pillars, and each pillar includes best practices and a set of guiding questions for cloud architectures.  

This section highlights the most relevant pillars for compute:  
- **Security**  
- **Performance efficiency**  
- **Cost optimization**  
- **Sustainability**  

---

### Best practice approach: Infrastructure protection – Protecting compute  
**Best practice:** *Automate compute protection.*  

Compute resources (such as EC2 instances) require multiple layers of defense. Automating protection reduces human error and frees resources for other security areas.  

**Supporting EC2 features:**  
- **EC2 Image Builder** – reduces exposure to vulnerabilities.  
- **User data scripts** – automate initialization tasks.  
- **Silver (mutable) and Golden (immutable) AMIs** – enforce security configuration.  

---

### Best practice approach: Infrastructure protection – Protecting networks  
**Best practice:** *Control traffic at all layers.*  

Workloads should be secured at every connectivity layer.  

**Key practices:**  
- Separate workloads into **different AWS accounts** for compliance and sensitivity.  
- Use **VPCs** to define network topology with IPv4 or IPv6.  
- Apply **defense in depth** using multiple controls for inbound and outbound traffic.  
- Use **security groups** to define permitted ports and protocols per workload.  

---

### Best practice approach: Compute and hardware  
**Best practices:**  
- *Scale the best compute options for your workload.*  
- *Configure and right-size compute resources.*  

Choosing the right compute ensures efficiency and performance.  

**Supporting EC2 considerations:**  
- Select **AMIs** with required software and correct root storage type.  
- Choose the correct **instance type and size**.  
- Configure **Amazon EBS or Instance Store** volumes as appropriate.  

---

### Best practice approach: Cost effective resourcing  
**Best practices:**  
- *Select the correct resource type, size, and number.*  
- *Select the best pricing model.*  

Efficient cost management ensures workloads are both effective and economical.  

**AWS pricing and capacity options include:**  
- **On-Demand Instances** – flexible, pay-per-use.  
- **Reserved Instances** – long-term savings with commitment.  
- **Spot Instances** – cost savings for flexible workloads.  
- **Savings Plans** – flexible commitment model.  
- **Dedicated options** – e.g., On-Demand Capacity Reservations, EC2 Capacity Blocks for ML, Dedicated Hosts.  

---

### Best practice approach: Hardware and services (Sustainability)  
**Best practices:**  
- *Use the minimum amount of hardware to meet your needs.*  
- *Use instance types with the least impact.*  
- *Use managed services.*  

The sustainability pillar emphasizes reducing environmental impact:  
- **Right-size resources** to avoid waste.  
- Adopt **efficient instance types** and monitor new releases for improvements.  
- Use **managed services** (AWS Batch, AWS Outposts) to leverage AWS efficiencies.  

**Storage considerations:**  
- Use **instance store volumes** for temporary, fast storage.  
- Use **Amazon EBS** for persistent storage needs.  

---

### Key takeaways: Applying AWS Well-Architected Framework principles to compute  
- Automate compute protection.  
- Scale the best compute options for your workload.  
- Configure and right-size compute resources.  
- Select the correct resource type, size, and number.  
- Select the best pricing model.  
- Use the minimum amount of hardware to meet your needs.  
- Use instance types with the least impact.  


---

## Module 5 Summary: Adding a Compute Layer Using Amazon EC2  
This section summarizes what you learned and brings the module to a close.  
*AWS Training and Certification Module 5: Adding a Compute Layer Using Amazon EC2 © 2024, Amazon Web Services, Inc. or its affiliates. All rights reserved.*  

---

### This module prepared you to:  
- Identify how **Amazon Elastic Compute Cloud (Amazon EC2)** can be used in an architecture.  
- Explain the value of using **Amazon Machine Images (AMIs)** to accelerate creation and repeatability of infrastructure.  
- Recommend **EC2 instance types** based on requirements.  
- Recommend **storage solutions** for Amazon EC2.  
- Recognize how to configure **Amazon EC2 instances with user data**.  
- Describe **EC2 pricing options** and recommend based on cost.  
- **Launch an Amazon EC2 instance.**  
- Apply the **AWS Well-Architected Framework principles** when designing a compute layer with Amazon EC2.  
