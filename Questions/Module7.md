## VPC Networking and Security Compilation

---

### Question 5

**Question 5:** A group of consultants requires access to an EC2 instance from the internet for 3 consecutive days each week. The instance is shut down the rest of the week. The virtual private cloud (VPC) has internet access. How should you assign one IPv4 address to the instance to give the consultants access?

*   Associate an Elastic IP address with the EC2 instance.
*   Enable automatic address assignment for the subnet.
*   Assign the IP address in the operating system (OS) boot configuration.
*   Enable automatic address assignment for the EC2 instance.

**Answer:**

*   **Associate an Elastic IP address with the EC2 instance.**

**Rationale:** An **Elastic IP address** is a **static, public IPv4 address**. Since automatically assigned public IPv4 addresses would typically change when an EC2 instance is stopped and restarted, using an Elastic IP ensures the consultants have a consistent address for connectivity.

---

### Question 6

**Question 6:** An application uses a bastion host to allow access to EC2 instances in a private subnet within a virtual private cloud (VPC). What security group configurations would allow SSH access from the source IP to the EC2 instances? (Select TWO.)

*   [x] **Add a rule to the bastion host security group to allow traffic on port 22 from your source IP address.**
*   [ ] Add a rule to the bastion host security group to deny all traffic from the internet.
*   [x] **Add a rule to the EC2 instance security group to allow traffic from the bastion host security group on port 22.**
*   [ ] Add a rule to the private subnet EC2 instance security group to allow return traffic to the bastion host security group.
*   [ ] Add a rule to the bastion host security group to allow return traffic to your source IP address.

**Rationale:** The bastion host (Security Group A) must be configured to allow SSH TCP traffic on port 22 from the specified IP address range. The private subnet EC2 instance (Security Group B) must then allow SSH traffic originating from Security Group A. Security groups are **stateful** firewalls, meaning return traffic is automatically allowed, making explicit return traffic rules (D and E) unnecessary. Security groups also **cannot specify deny rules** (B).

---

### Question 7

**Question 7:** A solution deployed in a virtual private cloud (VPC) needs a subnet with limited access to specific internet addresses. How can an architect configure the network to limit traffic from and to the EC2 instances in the subnet using a network access control list (ACL)?

*   Add rules to the default network ACL to allow traffic from and to allowed internet addresses.
*   **Add rules to the subnet custom network ACL to allow traffic from and to allowed internet addresses.**
*   Add rules to the default network ACL to allow traffic from and to allowed internet addresses. Deny all other traffic.
*   Add rules to the subnet custom network ACL to allow traffic from and to allowed internet addresses. Deny all other traffic.

**Rationale:** A custom network ACL should be used to restrict traffic at the subnet level. Since Network ACLs are **stateless**, rules must explicitly allow traffic in both the inbound and outbound directions ("from and to"). A **custom network ACL** automatically includes a deny-all asterisk (\*) rule, meaning only the necessary ALLOW rules need to be added to enforce the limitation, making an explicit final deny rule redundant.

---

### Question 8

**Question 8:** Which actions are best practices for designing a virtual private cloud (VPC)? (Select THREE.)

*   [x] **Reserve some address space for future use.**
*   [ ] Use the same Classless Inter-Domain Routing (CIDR) block as your on-premises network.
*   [ ] Use the same Classless Inter-Domain Routing (CIDR) block for subnets in different Availability Zones that are part of the same AWS Auto Scaling group.
*   [ ] Match the size of the VPC Classless Inter-Domain Routing (CIDR block to the number of hosts that are required for a workload.
*   [x] **Create one subnet per Availability Zone for each group of hosts that have unique routing requirements.**
*   [x] **Divide the VPC network range evenly across all Availability Zones that are available.**

**Rationale:** Best practices include designing a network that **can handle anticipated growth in traffic** (reserving space). For **resilience against AZ failures**, resources must be distributed across multiple Availability Zones (AZs). Since a subnet must belong to only **one AZ**, the standard design is to create one subnet per AZ for each necessary routing type, and to ensure this is possible, the VPC network range must be distributed across all available AZs.

---

### Question 9

**Question 9:** Where can you have VPC flow logs delivered? (Select THREE.)

*   [x] **Amazon Kinesis Data Firehose**
*   [ ] Amazon OpenSearch Service
*   [ ] Amazon Athena
*   [x] **Amazon S3 bucket**
*   [ ] AWS Management Console
*   [x] **Amazon CloudWatch**

**Rationale:** Amazon VPC Flow Logs can be delivered to three destinations: **Amazon CloudWatch Logs** (viewable in the AWS Management Console), **Amazon S3** (queryable with Amazon Athena), or **Amazon Kinesis Data Firehose** (for delivery to services like Amazon OpenSearch Service).

---

### Question 10

**Question 10:** An EC2 instance must connect to an Amazon S3 bucket. What component provides this connectivity with no additional charge and no throughput packet limits?

*   Public region access point
*   Interface VPC endpoint
*   Gateway Load Balancer endpoint
*   **Gateway VPC endpoint**

**Answer:**

*   **Gateway VPC endpoint**

**Rationale:** **Gateway VPC endpoints** provide direct connectivity to **Amazon S3** using route tables. They have **no additional charge** and **no throughput or packet size limits**. Interface VPC endpoints, while also providing connectivity, **incur costs** for hourly usage and data processed.
