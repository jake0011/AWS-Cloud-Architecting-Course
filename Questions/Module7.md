## VPC Networking and Security Compilation

This document compiles the questions and answers regarding AWS Virtual Private Cloud (VPC) components, security, and connectivity, along with a brief rationale supported by the provided sources.

---

### 5. Elastic IP Addresses

**Question:** A group of consultants requires access to an EC2 instance from the internet for 3 consecutive days each week. The instance is shut down the rest of the week. The virtual private cloud (VPC) has internet access. How should you assign one IPv4 address to the instance to give the consultants access?

**Answer:** Associate an Elastic IP address with the EC2 instance.

**Rationale:** An Elastic IP address is defined as a **static, public IPv4 address**. Since the EC2 instance is stopped and restarted weekly, using an Elastic IP is necessary because automatically assigned public IPv4 addresses would change upon restart, disrupting access.

---

### 6. Bastion Host Security Group Configuration

**Question:** An application uses a bastion host to allow access to EC2 instances in a private subnet within a virtual private cloud (VPC). What security group configurations would allow SSH access from the source IP to the EC2 instances? (Select TWO.)

**Answer (Select TWO):**
1. Add a rule to the bastion host security group to allow traffic on port 22 from your source IP address.
2. Add a rule to the EC2 instance security group to allow traffic from the bastion host security group on port 22.

**Rationale:** The bastion host (Security Group A) must allow SSH (port 22) traffic from the specific administrative IP range. The private EC2 instance (Security Group B) must then allow SSH (port 22) only from the bastion host's Security Group A, establishing a secure path. Security groups are **stateful**, meaning return traffic is automatically allowed, eliminating the need for explicit outbound return rules.

---

### 7. Network ACL Configuration

**Question:** A solution deployed in a virtual private cloud (VPC) needs a subnet with limited access to specific internet addresses. How can an architect configure the network to limit traffic from and to the EC2 instances in the subnet using a network access control list (ACL)?

**Answer:** Add rules to the subnet custom network ACL to allow traffic from and to allowed internet addresses.

**Rationale:** Network ACLs operate at the **subnet level** and are used to allow or deny specific traffic. A custom network ACL is used for unique subnet restrictions. Since ACLs are **stateless**, rules must explicitly allow both inbound and outbound traffic ("from and to") for the allowed addresses. The custom network ACL inherently contains a deny-all asterisk (\*) rule, which automatically denies all traffic not explicitly allowed.

---

### 8. VPC Design Best Practices

**Question:** Which actions are best practices for designing a virtual private cloud (VPC)? (Select THREE.)

**Answer (Select THREE):**
1. Reserve some address space for future use.
2. Create one subnet per Availability Zone for each group of hosts that have unique routing requirements.
3. Divide the VPC network range evenly across all Availability Zones that are available.

**Rationale:** Best practices include designing the network to **handle anticipated growth** (reserving address space). Subnets must belong to **one Availability Zone (AZ)** and serve as containers for routing policies; thus, subnets should be replicated across multiple AZs (creating one subnet per AZ for routing groups) to ensure the network is **resilient to failure**. Distributing the VPC network range across all available AZs supports this resilience.

---

### 9. VPC Flow Log Delivery Destinations

**Question:** Where can you have VPC flow logs delivered? (Select THREE.)

**Answer (Select THREE):**
1. Amazon Kinesis Data Firehose
2. Amazon S3 bucket
3. Amazon CloudWatch

**Rationale:** VPC Flow Logs, which capture packet-level traffic information, can deliver logs to three destinations: **Amazon CloudWatch Logs** (viewable in the console), **Amazon S3** (queryable with Amazon Athena), or **Amazon Kinesis Data Firehose** (for ingestion into services like Amazon OpenSearch Service or third-party solutions).

---

### 10. S3 Connectivity Component

**Question:** An EC2 instance must connect to an Amazon S3 bucket. What component provides this connectivity with no additional charge and no throughput packet limits?

**Answer:** Gateway VPC endpoint

**Rationale:** **Gateway VPC endpoints** provide direct connectivity to **Amazon S3** (and DynamoDB) using route tables. They are explicitly noted to have **no additional charge** and **no throughput or packet size limits**. This contrasts with Interface VPC endpoints, which incur hourly costs and have throughput limitations.
