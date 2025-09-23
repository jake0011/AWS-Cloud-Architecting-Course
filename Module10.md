# Module 10: Implementing Monitoring, Elasticity, and High Availability
This module focuses on designing AWS architectures that ensure monitoring, scalability, and high availability using services like Amazon CloudWatch, Amazon EventBridge, Amazon EC2 Auto Scaling, load balancers, Amazon Route 53, and the AWS Well-Architected Framework. Below, we outline the introduction, objectives, overview, labs, and key considerations for building performant and resilient systems.

## Introduction
This section introduces the content of the module, emphasizing the importance of monitoring, elasticity, and high availability in cloud architectures to meet performance and reliability requirements.

- **Module Objectives**:
  - Examine how reactive architectures use **Amazon CloudWatch** and **Amazon EventBridge** to monitor metrics and facilitate notification events.
  - Use **Amazon EC2 Auto Scaling** to promote elasticity and create highly available environments.
  - Determine strategies to scale **database resources**.
  - Identify a **load balancing strategy** to ensure high availability.
  - Use **Amazon Route 53** for DNS failover to maintain availability during failures.
  - Apply **AWS Well-Architected Framework principles** when designing highly available systems.

- **Module Overview**:
  - **Presentation Sections**:
    - Monitoring your resources
    - Scaling your compute resources
    - Scaling your databases
    - Using load balancers to create highly available environments
    - Using Route 53 to create highly available environments
    - Applying AWS Well-Architected Framework principles to highly available systems
  - **Demos**:
    - Creating Scaling Policies for Amazon EC2 Auto Scaling
    - Creating a Highly Available Application
    - Amazon Route 53: Simple Routing
    - Amazon Route 53: Failover Routing
    - Amazon Route 53: Geolocation Routing
  - **Knowledge Checks**:
    - 10-question knowledge check delivered in the online course
    - Sample exam question for class discussion

- **Hands-on Labs in This Module**:
  - **Guided Lab**:
    - **Creating a Highly Available Environment**: Step-by-step instructions to configure a resilient architecture.
  - **Challenge (Café) Lab**:
    - **Creating a Scalable and Highly Available Environment for the Café**: Update the café’s architecture to ensure scalability and availability.
    - Additional details are in the student guide, with instructions provided in the lab environment.

- **Cloud Architect Considerations**:
  - Design workload monitoring across distributed components to provide a complete picture of performance.
  - Automate scaling of compute and database resources to ensure high availability.
  - Implement failover mechanisms using load balancers and network routing to maintain availability during failures.
  - Work backward from business needs (e.g., the fictional café scenario) to design architectures tailored to specific use cases.

---

## Section 2: Monitoring Your Resources
This section explores how to monitor resources in AWS, focusing on distributed applications, Amazon CloudWatch for metrics and logs, alarms, dashboards, Amazon EventBridge for event routing, and cost monitoring tools. Below, we detail the key concepts and best practices for ensuring operational health, resource utilization, and application performance.

- **Monitoring Distributed Application Components**:
  - A responsive system requires distributed components to respond timely under heavy load, with attention to latency (e.g., synchronous services must respond within time limits).
  - Low-latency error-handling mechanisms are needed to report issues for remedial action.
  - For message-passing applications, consolidate separate service logs into a central service for complete application logs, aggregating metrics and setting alarms for threshold breaches.
  - Monitored metrics include:
    - **Operational health metrics**: Ensure runtime environment stability.
    - **Resource utilization metrics**: Prevent over- or underutilization of components.
    - **Application performance metrics**: Verify demand is met.

- **Amazon CloudWatch**:
  - A metrics and logs repository that collects and tracks metrics for AWS services across Regions.
  - Uses Amazon CloudWatch Logs to monitor, store, and access logs from EC2 instances, AWS CloudTrail, Amazon Route 53, and other services.
  - Supports built-in AWS service metrics or custom metrics.
  - Calculates statistics (metric data aggregations over time) and displays graphs.
  - Provides alarms for event-driven architectures and notifications to adjust monitored resources.
  - Supports cross-Region aggregation and hybrid/multicloud metrics querying in a single dashboard.

- **CloudWatch Metric and Log Collection**:
  - AWS resources send operational logs to CloudWatch Logs for centralized storage and access.
  - Centralizes logs from systems, applications, and AWS services into a scalable flow of events ordered by time.
  - Features querying with a powerful language, auditing/masking sensitive data, and generating metrics from logs via filters or embedded formats.
  - Custom data from other sources can be sent to CloudWatch Logs and the metric repository.
  - Metrics are stored in namespace containers (e.g., AWS/EC2 for EC2); namespaces isolate metrics.
  - Metrics are time-ordered data points with units (e.g., bytes, seconds); dimensions (e.g., InstanceId) filter metrics.
  - Resolutions: Standard (1-minute granularity) or high (1-second granularity).

- **CloudWatch Alarms**:
  - Watch a single metric over a time period and invoke actions (e.g., SNS notifications, Auto Scaling policies) based on threshold breaches.
  - Invoke actions only for sustained state changes (e.g., CPUUtilization >70% for 5 minutes).
  - Evaluation periods: Compare multiple data points (e.g., three periods); notify only if breaches persist.
  - High-resolution alarms: 10/30-second periods for high-resolution metrics (higher charge); regular alarms: multiples of 60 seconds for standard metrics.

- **CloudWatch Dashboards**:
  - Create customized or automatic dashboards to monitor resources across Regions in a single view.
  - Display metrics and alarms; customize colors for metric tracking across graphs.
  - Support operational playbooks for incident response and shared views for team communication.
  - Query CloudWatch statistics for custom dashboards outside the console.

- **Amazon EventBridge**:
  - A serverless service for building event-driven applications by connecting components via events.
  - Uses event buses (routers for many-to-many routing) and pipes (point-to-point integrations with transformation/enrichment).
  - Rules match events (via event patterns or schedules with EventBridge Scheduler) and route to targets.
  - Supports up to five targets per rule; targets can be in other accounts/Regions.
  - Example: EventBridge Scheduler sends Event 1 to default bus, invoking Rule 1 to SNS; Event 2 invokes Rule 2 (to CloudWatch Logs) and Rule 3 (to Lambda) in parallel.
  - Event Pattern Example: Matches all EC2 instance-termination events:
    ```json
    {
      "source": ["aws.ec2"],
      "detail-type": ["EC2 Instance State-change Notification"],
      "detail": {
        "state": ["terminated"]
      }
    }
    ```

- **Monitoring Your Resource Costs**:
  - **AWS Cost Explorer**: Visualizes, understands, and manages costs/usage with daily/monthly granularity; views up to 13 months of data for spending patterns.
  - **AWS Budgets**: Sets custom budgets with alerts for costs/usage exceeding (or forecasted to exceed) budgeted amounts.
  - **AWS Cost and Usage Report**: Provides comprehensive cost/usage data, including metadata on services, pricing, and reservations.

- **Key Takeaways: Monitoring Your Resources**:
  - CloudWatch alarms send notifications to Amazon EC2 Auto Scaling and SNS topics.
  - CloudWatch collects logs and metrics from AWS services across Regions.
  - Use CloudWatch dashboards to visualize metrics and alarms.
  - EventBridge processes and routes events with an event bus or a pipe.
  - AWS Cost Explorer, AWS Budgets, and AWS Cost and Usage Report help understand and manage AWS infrastructure costs.

---

## Section 3: Scaling Your Compute Resources.
This section explores scaling compute resources in an AWS Virtual Private Cloud (VPC) to build reactive architectures that ensure elasticity, resilience, and responsiveness. It focuses on Amazon EC2 Auto Scaling, scaling mechanisms, and additional AWS scaling services, with a demo on creating EC2 Auto Scaling policies.

- **The Need for Reactive Architectures**:
  - Modern applications must handle **petabytes of data**, deliver **sub-second response times**, and maintain **near 100% uptime**.
  - Reactive architectures are elastic, resilient, responsive, and message-driven to meet these demands:
    - **Elastic**: Dynamically add/remove resources to avoid downtime or bottlenecks.
    - **Resilient**: Recover from load spikes, attacks, or component failures.
    - **Responsive**: Deliver low-latency responses under varying workloads.
    - **Message-Driven**: Use asynchronous message-passing for loose coupling, isolation, and **location transparency** (accessing data without knowing its location).
  - These characteristics ensure scalability and responsiveness without manual intervention.

- **Achieve Elasticity with Scaling**:
  - **Elasticity**: The ability of infrastructure to expand/contract as capacity requirements change.
  - **Scaling**: Increases/decreases compute capacity to achieve elasticity.
    - **Vertical Scaling**: Replaces a resource with a different-sized one (e.g., upgrading an EC2 instance to a larger type with more RAM/CPU). Requires data transfer, potentially causing downtime, and is hardware-limited, making it less cost-efficient or highly available.
    - **Horizontal Scaling**: Adds/removes resources (e.g., adding EC2 instances). Known as **scaling out** (adding) or **scaling in** (removing). Automatically transfers applications/data, ideal for internet-scale, elastic cloud applications.

- **Amazon EC2 Auto Scaling**:
  - Manages a logical collection of EC2 instances called an **Auto Scaling group**, spanning **Availability Zones** for high availability.
  - **Features**:
    - Automatically launches/terminates EC2 instances based on **launch templates** or configurations.
    - Scales based on **scaling policies**, **Elastic Load Balancing (ELB)** health checks, or **scheduled actions**.
    - Integrates with ELB to register new instances and receive health notifications.
    - Balances instances across Availability Zones.
    - Free of charge.
  - **Benefits**:
    - **Fault Tolerance**: Detects unhealthy instances, terminates them, and launches replacements, using multiple Availability Zones to handle zone failures.
    - **Availability**: Maintains capacity to handle current traffic demand.
    - **Cost Management**: Dynamically adjusts capacity to launch instances only when needed, reducing costs for unused resources.

- **Amazon EC2 Auto Scaling Group Components**:
  - **Capacity Settings**:
    - **Minimum Capacity**: Smallest number of instances needed to run the application.
    - **Maximum Capacity**: Largest number of instances permitted.
    - **Desired Capacity**: Optimal number of instances under normal conditions, adjusted manually or automatically within min/max limits.
  - **Launch Template**: Specifies instance configuration (e.g., instance type, AMI ID) and instance types (On-Demand, Reserved, Spot) with cost preferences.
  - **Scaling Mechanisms**: Added via scheduled actions, dynamic policies, or predictive policies.
  - Integrates with load balancers for instance registration and health checks.

- **Horizontal Scaling with Amazon EC2 Auto Scaling Groups**:
  - Example: Group with desired capacity = 2, max = 3, min = 1.
    - Starts with 2 instances (e.g., instance 1 and 2).
    - Scales out to 3 instances (adds instance 3) if a metric (e.g., CloudWatch CPU usage) triggers a policy, respecting the max capacity.
    - Scales in to 1 instance (e.g., terminates instances 1 and 2) during off-peak hours via a scheduled policy, respecting the min capacity.
    - Replaces unhealthy instances reported by ELB or group health checks.

- **Amazon EC2 Auto Scaling Mechanisms**:
  - **Scheduled Actions**: Scale based on predictable workloads (e.g., increasing instances on Wednesdays for weekly traffic spikes).
  - **Dynamic Policies**:
    - Scale based on tracked metrics (e.g., CloudWatch CPU utilization, SQS queue depth).
    - **Target Tracking Scaling**: Maintains a target metric value (e.g., 50% CPU utilization) by adjusting capacity, similar to a thermostat.
    - **Step Scaling**: Adjusts capacity based on the size of a metric breach (e.g., add/remove instances based on CPU utilization ranges).
    - **Simple Scaling**: Adjusts capacity and waits for completion before responding to new alarms.
    - Ideal for moderately spiky workloads.
  - **Predictive Policy**: Uses machine learning to analyze historical traffic patterns and proactively scale capacity for predictable cyclical or recurring workloads (e.g., high usage during business hours, batch processing).

- **Step Scaling Policy Example**:
  - Adjusts capacity based on metric value ranges:
    | Metric Value | Capacity Change |
    |--------------|-----------------|
    | <30%         | -30%            |
    | 30–39%       | -10%            |
    | 40–60%       | No change       |
    | 61–70%       | +10%            |
    | 71–100%      | +30%            |
  - Responds to ongoing alarms, scaling dynamically based on breach size.
  - CloudWatch aggregates metric data to trigger the policy.

- **Additional AWS Scaling Options**:
  - **AWS Auto Scaling**:
    - Uses a **scaling plan** to configure auto scaling for multiple resources (e.g., by tags for production/testing).
    - Supports:
      - **Amazon Aurora**: Scales read replicas.
      - **Amazon EC2 Auto Scaling**: Scales EC2 instances.
      - **Amazon ECS**: Scales task counts.
      - **Amazon DynamoDB**: Scales read/write capacity.
  - **AWS Application Auto Scaling**:
    - Scales resources beyond EC2 with target tracking, step, or scheduled scaling.
    - Supports:
      - AWS Auto Scaling services.
      - AWS Lambda functions.
      - Amazon SageMaker.
      - Amazon ElastiCache for Redis.

- **Demo: Creating Scaling Policies for Amazon EC2 Auto Scaling**:
  - Demonstrates:
    - Creating and launching an EC2 instance.
    - Creating an Auto Scaling group.
    - Configuring a scaling policy.
  - Available as a recorded demo in the online course.

- **Key Takeaways: Scaling Your Compute Resources**:
  - Amazon EC2 Auto Scaling manages EC2 instances in an **Auto Scaling group** with min, max, and desired capacity settings.
  - Scales in/out using scheduled actions, dynamic policies (target tracking, step, simple), or predictive policies.
  - AWS Auto Scaling and Application Auto Scaling extend scaling to services like Aurora, ECS, DynamoDB, Lambda, SageMaker, and ElastiCache.

---
## Section 4: Scaling Your Databases.
This section explores how to scale AWS database services, including Amazon Aurora, Aurora Serverless, Amazon RDS, and DynamoDB, to meet workload demands. It covers vertical and horizontal scaling, auto-scaling mechanisms, and service-managed storage scaling, ensuring elasticity and high availability for database resources.

- **Scaling AWS Databases**:
  - AWS database services support **vertical scaling** (increasing resource size) and **horizontal scaling** (adding/removing resources), with some offering manual and others auto-scaling options.
  - Scaling is not instantaneous but often managed as background processes for minimal disruption.
  - Key services and their scaling capabilities:
    - **Amazon Aurora**: Scales clusters vertically (instance size) and horizontally (read replicas); service-managed storage scaling.
    - **Amazon Aurora Serverless**: Scales compute horizontally with Aurora Compute Units (ACUs); service-managed storage scaling.
    - **Amazon RDS**: Scales databases vertically (instance size, storage) and horizontally (read replicas).
    - **DynamoDB**: Scales tables horizontally via read/write capacity units (RCUs/WCUs); service-managed storage scaling.

- **Scaling an Aurora Cluster**:
  - **Vertical Scaling**:
    - Change the Aurora DB instance class to adjust compute and memory capacity.
    - Takes ~15 minutes; can be scheduled during maintenance windows to avoid downtime.
    - Minimize downtime in Multi-AZ setups by adding a replica with the desired instance class, synchronizing, and failing over.
  - **Horizontal Scaling**:
    - Use **Aurora Auto Scaling** to dynamically adjust the number of **Aurora Replicas** (read-only instances, up to 15 per cluster).
    - Scaling policy defines min/max replicas; adjusts based on CloudWatch metrics (e.g., workload or connectivity spikes).
    - Removes unnecessary replicas when demand decreases to save costs.
  - **Storage Scaling**: Managed automatically by AWS in the Aurora Cluster volume, spanning multiple Availability Zones.
  - Example: A cluster with a primary instance in Availability Zone 1 and replicas in Zones 2 and 3, each connected to Amazon EBS volumes, with data copied across zones.

- **Scaling an Aurora Serverless Cluster**:
  - Designed for **intermittent or unpredictable workloads** (e.g., retail sales events, reporting databases, dev/test environments).
  - **Horizontal Compute Scaling**:
    - Specify min/max **Aurora Compute Units (ACUs)** (each ACU ≈ 2 GiB memory, corresponding CPU, and networking).
    - Automatically scales compute resources within the ACU range based on workload needs.
    - Supports starting/stopping clusters for cost efficiency during idle periods.
  - **Storage Scaling**: Managed automatically by AWS, similar to standard Aurora clusters.
  - **Pricing**: Pay per second for active ACU usage; migrate between standard and serverless configurations.
  - Example: A Multi-AZ cluster with a 10 ACU writer in Zone 1 and 2 ACU readers in other zones, with EBS-backed cluster volume.

- **Scaling an Amazon RDS Database**:
  - **Vertical Scaling**:
    - Change the DB instance class to adjust compute/memory capacity.
    - Temporarily unavailable during scaling (Single-AZ) unless applied during maintenance windows; Multi-AZ minimizes downtime by upgrading standby first, then failing over.
    - Modify storage size or type (e.g., General Purpose SSD to Provisioned IOPS SSD) manually or use **RDS storage auto-scaling** for automatic storage increases.
  - **Horizontal Scaling**:
    - Add **read replicas** (read-only instances for MariaDB, MySQL, Oracle, PostgreSQL) to offload read traffic (e.g., for business reporting or read queries).
    - Updates from the source DB are asynchronously copied to replicas.
    - Replicas can be promoted to primary for disaster recovery but lack Multi-AZ’s automatic failover.
  - Example: A primary RDS instance with an EBS volume, replicating data asynchronously to a read replica with its own EBS volume.

- **Scaling a DynamoDB Table**:
  - Operates on **throughput capacity** (read capacity units - RCUs, write capacity units - WCUs).
  - **Capacity Modes**:
    - **On-Demand Mode**: Automatically scales RCUs/WCUs based on actual read/write traffic; ideal for spiky or unpredictable workloads.
    - **Provisioned Mode**: Set RCUs/WCUs manually; supports **DynamoDB auto-scaling** via Application Auto Scaling to adjust throughput dynamically based on traffic patterns.
      - Prevents throttling during traffic spikes and reduces capacity when demand drops.
      - Can be deactivated if not needed.
  - **Global Secondary Indexes (GSIs)**:
    - Have separate throughput capacity from the base table; auto-scaling adjusts GSIs to maintain desired utilization.
  - **Storage Scaling**: Managed automatically by AWS.
  - Example: Enable on-demand mode for a table to handle sudden traffic peaks without manual intervention.

- **Comparing AWS Database Scaling Mechanisms**:
  | **Service**          | **Scope**   | **Vertical Scaling**                     | **Horizontal Scaling**                          | **Managed by AWS** |
  |----------------------|-------------|-----------------------------------------|------------------------------------------------|---------------------|
  | **Aurora**           | Cluster     | Instance class/size                     | Aurora Auto Scaling for read replicas, Multi-AZ replicas | Storage scaling     |
  | **Aurora Serverless**| Cluster     | ACU throughput limits (auto-scaled)     | Reader/writer replicas within ACU limits         | Storage scaling     |
  | **Amazon RDS**       | Database    | Instance class/size, storage auto-scaling | Read replicas                                  | N/A                 |
  | **DynamoDB**         | Table       | N/A                                     | On-demand mode, provisioned mode with auto-scaling, GSIs | Storage scaling     |

- **Key Takeaways: Scaling Your Databases**:
  - **Aurora**: Scale vertically (instance class) or horizontally (up to 15 read replicas with Aurora Auto Scaling); AWS manages storage scaling.
  - **Aurora Serverless**: Automatically scales compute (ACUs) and storage for intermittent/unpredictable workloads.
  - **Amazon RDS**: Vertically scale compute (instance class) or storage (manual/auto-scaling); horizontally scale with read replicas.
  - **DynamoDB**: Use on-demand mode for automatic scaling or provisioned mode with auto-scaling for RCUs/WCUs; supports GSIs; AWS manages storage scaling.

---

## Section 5: Using Route 53 to Create Highly Available Environments
This section explores how **Amazon Route 53** enhances high availability through DNS management, routing policies, and health checks to ensure traffic is directed to healthy resources. It covers DNS lookups, Route 53 features, routing policies, public/private hosted zones, and multi-Region failover, with demos illustrating simple, failover, and geolocation routing.

- **DNS Lookups**:
  - **Domain Name System (DNS)**: Translates human-readable domain names (e.g., `www.myweb.com`) into IP addresses (e.g., `192.0.2.1`) for computers to connect.
  - Acts like a phone book, mapping names to numbers via DNS servers.
  - **Process**:
    1. User enters `www.myweb.com` in a browser; request routes to a DNS resolver (typically managed by the ISP).
    2. Resolver queries a DNS root name server, which returns the `.com` top-level domain (TLD) name server.
    3. Resolver queries the `.com` TLD server, which returns name servers for `myweb.com`.
    4. Resolver queries the `myweb.com` name server, which returns the IP address (e.g., `192.0.3.4`).
    5. Resolver caches the IP for faster future queries and returns it to the browser.
    6. Browser connects to the IP, and the server returns the webpage.
  - **Recursive DNS**: Acts as an intermediary, caching or fetching DNS data from authoritative servers.

- **Amazon Route 53**:
  - A highly available, scalable DNS web service with three main functions:
    - **Domain Registration**: Purchase and manage domain names (e.g., `myweb.com`) with automatic DNS configuration.
    - **DNS Routing**: Routes user requests to AWS resources (e.g., EC2, S3, ELB) or external infrastructure using authoritative name servers.
    - **Health Checking**: Monitors resource health (IP addresses, domains, CloudWatch alarms) to manage Availability Zone or Regional failovers.
  - **Features**:
    - Provides **hosted zones** (containers for DNS records) for domains and subdomains.
    - Supports **zonal autoshift** (via Route 53 Application Recovery Controller) to automatically shift traffic from an impaired Availability Zone to healthy ones during outages.
    - Integrates with **AWS Certificate Manager (ACM)** for SSL/TLS certificates.
    - Enables multi-Region high availability and fault tolerance.

- **Route 53 Routing Policies**:
  - Determines how Route 53 responds to DNS queries for high availability and performance:
    - **Simple Routing**: Routes traffic to a single resource (e.g., a web server) without special routing logic.
    - **Weighted Routing**: Assigns weights to multiple resources (same name/type) to control traffic distribution (e.g., for load balancing or testing new software versions).
    - **Latency Routing**: Routes to the AWS Region with the lowest latency for the user, using latency records across multiple Regions.
    - **Failover Routing**: Routes to a primary resource when healthy; switches to a secondary resource if unhealthy (e.g., S3 bucket or complex record tree).
    - **Geoproximity Routing**: Routes based on the geographic location of users/resources, using a bias to adjust the size of the routing region (requires Route 53 Traffic Flow).
    - **Geolocation Routing**: Routes based on user location (continent, country, or U.S. state) for localized content or restricted distribution.
    - **Multivalue Answer Routing**: Returns multiple healthy resource IP addresses (up to 8) for DNS-based load balancing; not a substitute for ELB.
    - **IP-based Routing**: Optimizes routing based on user-IP-to-endpoint mappings for performance or cost (e.g., routing users from a specific ISP).
  - Combine with DNS failover for low-latency, fault-tolerant architectures.

- **Route 53 Public and Private Hosted Zones**:
  - **Hosted Zone**: A container for DNS records defining traffic routing for a domain (e.g., `myweb.com`) and subdomains (e.g., `www.myweb.com`).
  - **Public Hosted Zone**: Routes internet traffic to resources (e.g., EC2-hosted website).
  - **Private Hosted Zone**: Routes traffic within an Amazon VPC (e.g., for internal applications).
  - **Record Types**:
    - **A**: Maps to an IPv4 address.
    - **AAAA**: Maps to an IPv6 address.
    - **CNAME**: Maps a name to another domain/subdomain.
    - **MX**: Specifies mail servers and priority.
    - **NS**: Identifies name servers for the hosted zone.
  - **Route 53 Resolver**: Automatically answers DNS queries for:
    - Local VPC domain names (e.g., `ec2-192-0-2-44.compute-1.amazonaws.com`).
    - Private hosted zone records (e.g., `acme.example.com`).
    - Public domain name lookups.

- **Multi-Region Failover**:
  - **Purpose**: Routes traffic from an unhealthy resource to a healthy one across Regions for high availability.
  - **Active-Passive Failover**:
    - Primary resource(s) handle traffic when healthy; secondary resource(s) activate if all primaries are unhealthy.
    - Configured by setting the **failover routing policy** with primary and secondary records.
    - Example: If Application Load Balancer 1 (Region A) is unhealthy, Route 53 routes traffic to Application Load Balancer 2 (Region B).
  - **Health Checks**:
    - Monitor resource health (endpoint, calculated, or CloudWatch alarm-based).
    - **Route 53 Application Recovery Controller**: Uses routing controls (on/off switches) with failover DNS records to manage and coordinate failover across Availability Zones or Regions.
  - **Configurations**:
    - **Simple**: Group records (same name/type, e.g., A records for `example.com`) with health checks; Route 53 responds based on resource health.
    - **Complex**: Use a tree of records (e.g., latency alias records with weighted records per Region) to route based on multiple criteria (e.g., latency, instance type).

- **Demos**:
  - **Amazon Route 53: Simple Routing**:
    - Configures Route 53 to evenly distribute HTTP requests between two EC2 instances.
    - Defines a Route 53 record set.
  - **Amazon Route 53: Failover Routing**:
    - Uses Route 53 to direct traffic to two Availability Zones for failover.
    - Defines a Route 53 health check and sends alerts to an administrator.
  - **Amazon Route 53: Geolocation Routing**:
    - Uses geolocation routing to serve traffic based on user location.
    - Localizes content for geographic user groups.
  - Available as recorded demos in the online course.

- **Key Takeaways: Using Route 53 to Create Highly Available Environments**:
  - Route 53 is a DNS service that:
    - Manages domain name registrations.
    - Provides hosted zones and authoritative name servers.
    - Performs DNS routing and health checks for high availability.
  - Supports multiple routing policies:
    - Simple, Weighted, Latency, Failover, Geoproximity, Geolocation, Multivalue Answer, IP-based.
  - Enables public/private hosted zones for internet or VPC traffic routing.
  - Facilitates multi-Region failover with active-passive configurations and health checks to ensure traffic routes to healthy resources.

---

# Applying AWS Well-Architected Framework Principles to Highly Available Systems
This section explores how to apply the **AWS Well-Architected Framework** principles, specifically from the **Reliability** and **Performance Efficiency** pillars, to design highly available systems. It focuses on best practices for failure management, compute scaling, and automation to ensure systems remain available and resilient with minimal downtime.

- **AWS Well-Architected Framework Overview**:
  - Comprises six pillars: Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization, and Sustainability.
  - Each pillar includes best practices and questions to guide cloud solution architecture.
  - This section emphasizes **Reliability** (ensuring systems deliver functionality under failure) and **Performance Efficiency** (optimizing resource use) for high availability.
  - **High Availability (HA)**: Systems withstand degradation (e.g., component or zone failures) while remaining available with minimal downtime and human intervention.
  - For complete best practices, see the Well-Architected Framework website (linked in course resources).

- **Best Practice Approach: Failure Management – Use Fault Isolation to Protect Your Workload**:
  - **Objective**: Limit failure impact to specific components using **fault isolation boundaries**, preventing system-wide disruptions.
  - **Best Practices**:
    - **Deploy the Workload to Multiple Locations**:
      - Distribute workload data and resources across multiple **Availability Zones** or **AWS Regions** to avoid single points of failure.
      - Ensures resilience against failures of a single compute node, storage volume, database instance, or Availability Zone.
      - Example: Use **Amazon EC2 Auto Scaling** and **Amazon RDS Multi-AZ** to distribute instances and databases across zones.
      - AWS services are designed for independent and autonomous operation across Regions.
    - **Automate Recovery for Components Constrained to a Single Location**:
      - If multi-location deployment isn’t feasible due to constraints (e.g., specific IP requirements), automate infrastructure recreation, application redeployment, and data recovery.
      - Use **Amazon EC2 Auto Scaling groups** for instances/containers without fixed IP needs (e.g., no Elastic IP or instance metadata requirements).
      - Implement **self-healing automation** based on Amazon EC2 or ECS container lifecycle events for automatic recovery.
  - **Supporting AWS Services**:
    - **Amazon EC2 Auto Scaling**: Automatically replaces unhealthy instances across Availability Zones.
    - **Amazon RDS Multi-AZ**: Synchronously replicates data to a standby database for failover.

- **Best Practice Approach: Failure Management – Design Your Workload to Withstand Component Failures**:
  - **Objective**: Ensure workloads remain operational by routing traffic to healthy resources and notifying teams of issues.
  - **Best Practices**:
    - **Fail Over to Healthy Resources**:
      - Distribute load across resources, Availability Zones, or Regions to mitigate individual resource or location impairments.
      - Use systems (e.g., **Route 53**) to discover and route traffic to healthy resources during failures.
      - Patterns vary by service:
        - **Multi-AZ Services**: Lambda, Aurora, DynamoDB are inherently distributed.
        - **EC2/EKS**: Require specific designs for cross-zone failover.
      - Monitor failover health, progress, and business process recovery to ensure automatic or manual recovery to new resources.
      - Example: Route 53 failover routing shifts traffic to a healthy Application Load Balancer in another Region if the primary fails.
    - **Send Notifications When Events Impact Availability**:
      - Send alerts upon threshold breaches (e.g., error rates, latency, KPIs) via multiple channels, even if auto-healing resolves the issue.
      - Detect patterns in auto-healed events to address root causes and prevent recurrence.
      - Example: Use **CloudWatch** alarms to notify operations teams of breaches for rapid resolution.
  - **Supporting AWS Services**:
    - **Route 53**: Manages failover routing to healthy resources.
    - **CloudWatch**: Monitors metrics and triggers notifications for availability issues.
    - **Amazon EC2 Auto Scaling**: Replaces unhealthy instances to maintain availability.

- **Best Practice Approach: Compute and Hardware**:
  - **Objective**: Optimize compute resource allocation to match workload demands dynamically.
  - **Best Practices**:
    - **Scale Your Compute Resources Dynamically**:
      - Use AWS scaling mechanisms to adjust resources up/down based on demand, ensuring performance efficiency.
      - Combine with compute-related metrics (e.g., CPU utilization) to automatically respond to changes.
      - Ensure workloads handle both scale-up and scale-down events.
      - Scaling Approaches:
        - **Target-Tracking**: Monitors a metric (e.g., CPU utilization) and adjusts capacity to maintain a target value (e.g., 50% CPU).
        - **Predictive Scaling**: Uses machine learning to anticipate daily/weekly traffic trends and scale proactively.
        - **Schedule-Based**: Scales based on predictable load changes (e.g., peak hours).
        - **Service Scaling**: Uses serverless services (e.g., Lambda, Aurora Serverless) that scale automatically.
      - Example: Use **Amazon EC2 Auto Scaling** with target-tracking to maintain optimal instance counts.
  - **Supporting AWS Services**:
    - **Elastic Load Balancing (ELB)**: Distributes traffic to scaled instances.
    - **CloudWatch**: Provides metrics for scaling decisions.
    - **Amazon EC2 Auto Scaling**: Dynamically adjusts instance counts based on policies.

- **Key Takeaways: Applying AWS Well-Architected Framework Principles to High Availability**:
  - Deploy workloads to multiple Availability Zones or Regions to eliminate single points of failure.
  - Automate recovery for single-location components using EC2 Auto Scaling or self-healing automation.
  - Fail over to healthy resources using Route 53 for seamless traffic routing.
  - Send notifications via CloudWatch for threshold breaches to address issues promptly.
  - Dynamically scale compute resources with EC2 Auto Scaling, ELB, and serverless services to match demand.

---

### Module summary
This module prepared you to do the following: 
- Examine how reactive architectures use Amazon CloudWatch and Amazon EventBridgeto monitor metrics and facilitate notification events.
- Use Amazon EC2 Auto Scaling within an architecture to promote elasticity and create a highly available environment.
- Determine how to scale your database resources.
- Identify a load balancing strategy to create a highly available environment.
- Use Amazon Route 53 for DNS failover.
- Use the AWS Well-Architected Framework principles when designing highly available systems.
