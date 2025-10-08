### Question 1

What is the simplest way to connect 100 virtual private clouds (VPCs) together?

1. Chain VPCs together by using VPC peering.
2. Connect each VPC to all the other VPCs by using VPC peering.
3. Create a hub-and-spoke network by using AWS VPN CloudHub.
4. **Connect the VPCs to AWS Transit Gateway.**

**Rationale Note:**
AWS Transit Gateway utilizes a **hub-and-spoke architecture**, which is suitable for **large networks**. This architecture requires only **one connection per VPC** (100 connections for 100 VPCs), which significantly **simplifies management**. A full mesh using VPC peering for 100 VPCs would require 4,950 connections, resulting in **high operational overhead**. The AWS Well-Architected Framework principles recommend preferring **hub-and-spoke topologies** (Transit Gateway) over a many-to-many mesh architecture (VPC peering) to simplify management for multiple VPCs.

---

### Question 2

A company needs network traffic to flow between an AWS account in one Region to another account in a different Region. What should they set up between the transit gateways in each region?

1. AWS Direct Connect
2. **Transit gateway peering attachment**
3. AWS Site-to-Site VPN
4. AWS PrivateLink

**Rationale Note:**
AWS Transit Gateway supports **peering with other transit gateways in different AWS Regions or accounts**. Establishing a peering attachment enables **traffic flow between the transit gateways** located in separate regions. Traffic flowing across this peering attachment remains **secure on the AWS global backbone** and does **not traverse the public internet**.

---

### Question 3

A company has two virtual private clouds (VPCs). VPC A has a Classless Inter-Domain Routing (CIDR) block of 10.1.0.0/16. VPC B has CIDR block of 10.2.0.0/16. Both VPCs belong to the same AWS account. What is the simplest way to connect the two VPCs so that they can route all traffic between them?

1. **VPC peering**
2. AWS Site-to-Site VPN
3. VPC endpoints
4. AWS Direct Connect

**Rationale Note:**
**VPC peering** establishes a **one-to-one networking connection** between two VPCs to provide private network traffic routes. It is a **cost-effective method** ideal for a **small number of VPCs**. There is **no cost for creating VPC peering connections**. The CIDR blocks must **not overlap**, a condition met in this scenario.

---

### Question 4

Systems in a secure subnet in a virtual private cloud (VPC) must access a bucket in Amazon S3. Which solutions stop traffic from crossing the internet? (Select TWO.)

1. Use the private IP address of Amazon S3.
2. Use a private IP address for the system.
3. **[x] Create a VPC gateway endpoint for Amazon S3.**
4. **[x] Use VPC interface endpoints.**
5. Create a VPC peering connection to Amazon S3.

**Rationale Note:**
A core network design principle is to **protect data in transit between networks** to meet security compliance requirements. When using connectivity services that keep traffic within the AWS environment (like those provided by endpoints, VPC peering, or Transit Gateway), traffic remains **secure on the AWS global backbone** and avoids the public internet. These endpoint solutions fulfil the need to **control traffic at all layers**.

---

### Question 5

A company has three virtual private clouds (VPCs). VPCs A, B, and C have Classless Inter-Domain Routing (CIDR) blocks that do not overlap. Both A and C have separate VPC peering connections with B. However, A cannot communicate with C. What is the simplest and most cost-effective way to enable full communication between A and C?

1. Add routes to B to enable traffic between A and C through B.
2. Link all three VPCs through a transit VPC, and route all traffic through the transit VPC.
3. **Add a peering connection between A and C, and route traffic between A and C through the peering connection.**
4. Create VPC endpoints in A and C for the individual hosts that need to communicate with each other.

**Rationale Note:**
**VPC peering does not support transitive peering relationships**. Since peering is strictly **one-to-one**, VPC A must be directly peered with VPC C to communicate. Adding a new peering connection is the **simplest and most cost-effective** fix because there is **no cost for creating VPC peering connections**.

---

### Question 6

Because of a natural disaster, a company moved a secondary data center to a temporary facility with internet connectivity. It needs a secure connection to the company's virtual private cloud (VPC) that must be operational as soon as possible. The data center will move again in 2 weeks. Which option meets the requirements?

1. AWS Direct Connect
2. **AWS Site-to-Site VPN**
3. VPC peering
4. VPC endpoints

**Rationale Note:**
**AWS Site-to-Site VPN** creates a **secure connection** over the public internet by encrypting traffic. Crucially, VPN connections **can be set up quickly, typically within hours**, meeting the requirement to be operational **as soon as possible**. In contrast, **AWS Direct Connect** requires planning and physical resources, **typically spanning multiple weeks** for implementation.

---

### Question 7

A company is concerned about internet disruptions. It wants to efficiently route traffic from their on-premises network to an AWS edge location close to their customer gateway device. What should they use?

1. AWS VPN CloudHub
2. **AWS Global Accelerator**
3. AWS Transit Gateway
4. AWS Direct Connect

**Rationale Note:**
**AWS Global Accelerator** routes traffic from the on-premises network to the **nearest AWS edge location**, and then **over the AWS backbone** to a transit gateway. This practice is recommended to **optimize performance** and **reduce disruptions from public internet traffic**.

---

### Question 8

A company is implementing a system to back up on-premises systems to AWS. Which network connectivity method provides a solution with the most consistent performance?

1. Virtual private cloud (VPC) peering
2. **AWS Direct Connect**
3. Virtual private cloud (VPC) endpoints
4. AWS Site-to-Site VPN

**Rationale Note:**
**AWS Direct Connect** establishes a dedicated, private connection that offers **consistent network performance**, **increased bandwidth**, and **higher throughput**. Direct Connect is the ideal solution for **large data transfers** and for applications requiring **predictable performance** by helping to **reduce public internet congestion**.

---

### Question 9

A company uses a single AWS Direct Connect connection between their on-premises network and their virtual private cloud (VPC). They want to ensure that the network connectivity is highly available by adding a backup connection. Which network connectivity method provides the most cost-effective solution for the backup connection?

1. **An on-demand AWS Site-to-Site VPN connection across the internet**
2. Another AWS Direct Connect connection through a different Direct Connect location
3. Another AWS Direct Connect connection through the same Direct Connect location
4. An on-demand AWS Client VPN connection across the internet

**Rationale Note:**
A best practice for high availability is to combine **Direct Connect (primary)** with a **Site-to-Site VPN (backup)**. Using Site-to-Site VPN is the **most cost-effective** backup method, as VPN connections **charge per VPN connection-hour when provisioned and available**. This is cheaper than incurring the high investment and recurring expenses of a redundant dedicated physical Direct Connect circuit.

---

### Question 10

A company is connecting a virtual private cloud (VPC) to multiple on-premises data centers using a virtual private network (VPN). Which implementation ensures resiliency and predictable bandwidth requirements?

1. **Implement Direct Connect as the primary connection and use the VPN as a secondary failover connection from each data center.**
2. Use a many-to-many mesh topology, such as Amazon VPC peering.
3. Implement AWS Transit Gateway to connect to each on-premises data center.
4. Establish multiple Border Gateway Protocol (BGP) sessions for each VPC to create connectivity to multiple VPCs across multiple AWS Regions.

**Rationale Note:**
**AWS Direct Connect** provides the required **consistent/predictable performance**. The strategy of combining **Direct Connect (primary)** with a **Site-to-Site VPN (backup)** is a **best practice** for achieving high availability and **resiliency** in hybrid network environments. This ensures that traffic utilizes the reliable path (Direct Connect) while having continuous connectivity available via VPN failover.
