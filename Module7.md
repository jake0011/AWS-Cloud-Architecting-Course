# Module 7: Creating a Networking Environment
This module describes the customer usage of the AWS networking environment with the Amazon Virtual Private Cloud (VPC) service. Below, we dive into the key components and objectives of this module, ensuring you understand how to design, secure, and monitor a network environment in AWS.

## Introduction
  This section outlines the content of the module, focusing on the AWS networking environment and the role of Amazon Virtual Private Cloud (VPC).
- **Module Objectives**:
  - Explain the role of a virtual private cloud (VPC) in Amazon Web Services (AWS) Cloud networking.
  - Identify the components in a VPC that can connect an AWS networking environment to the internet.
  - Isolate and secure resources within your AWS networking environment.
  - Create and monitor a VPC with subnets, an internet gateway, route tables, and a security group.
  - Use the AWS Well-Architected Framework principles when creating and planning a network environment.
- **Module Overview**:
  - **Presentation sections**: Introducing Amazon VPC, Securing network resources, Connecting to managed AWS services, Monitoring your network, Applying AWS Well-Architected Framework principles to a network.
  - **Demo**: Creating an Amazon VPC in the AWS Management Console.
  - **Activity**: Choose the Right Type of Subnet.
  - **Knowledge checks**: 10-question knowledge check and a sample exam question.
- **Hands-on Labs**:
  - **Guided lab**: Creating a Virtual Private Cloud.
  - **Challenge (Café) lab**: Creating a VPC Networking Environment for the Café.
- **Cloud Architect Considerations**:
  - Design a network that’s resilient to failure and can handle anticipated growth in traffic so that it’s available when needed.
  - Secure the network effectively to provide access to users and applications that should have it while preventing unwanted traffic.
  - Understand how network design decisions impact performance and cost to optimize the value of the network to the business.

---

## Section 2: Introducing Amazon VPC
This section introduces the Amazon Virtual Private Cloud (VPC) service and its components. Below, we outline the key aspects of AWS's physical and virtual networking infrastructure, focusing on how Amazon VPC enables you to create a secure, scalable, and customizable network environment.

- **AWS Physical Infrastructure**:
  - AWS Cloud infrastructure resides in data centers containing thousands of servers built into racks, with network routers and switches to route traffic.
  - Data centers are grouped into Availability Zones (AZs), connected with single-digit millisecond latency networks.
  - AZs are grouped into an AWS Region, with latency between Regions in the tens of milliseconds.
  - Each AWS Region is a separate geographic area located in a country, and each AZ consists of one or more data centers with redundant power, networking, and connectivity.
  - Inside a data center, physical network devices like routers, switches, firewalls, and load balancers are connected to each rack, with each device having an IP address for routing.
  - A virtual network (overlay network) emulates a physical network with software-defined components like switches, routers, firewalls, and load balancers, created and managed programmatically.

- **AWS Account Resource Isolation**:
  - When you request a new AWS account, network resources are dedicated to the account, spanning across AWS Regions.
  - Each account contains a default virtual, software-defined network (Amazon VPC) in each publicly accessible Region, which is logically isolated from other virtual networks and the internet.
  - AWS services like Amazon EC2 and Amazon RDS operate inside a VPC, while serverless services like AWS Lambda and Amazon CloudWatch operate outside a customer VPC.
  - The default VPC has built-in connectivity configurations, so it’s not recommended for production workloads; instead, define a new VPC with appropriate security and connectivity settings.

- **Amazon Virtual Private Cloud (Amazon VPC)**:
  - A programmatically defined, logically isolated virtual network similar to a traditional data center network, belonging to one Region.
  - Customizable to control traffic flow to and from the VPC, sized by a range of private IP addresses called a CIDR block.
  - The maximum IPv4 VPC size is a /16 netmask with 65,536 IP addresses, and the smallest is a /28 netmask with 16 IP addresses.
  - VPCs can be IPv4 only or dual stack (IPv4 and IPv6 CIDR blocks). IPv6 offers a larger address space (128 bits vs. 32 bits for IPv4) and better speed performance due to no NAT.
  - Amazon VPC IP Address Manager (IPAM) helps plan, track, and monitor IP addresses efficiently.
  - Note: Effective early 2024, there is a per-hour cost for all public IPv4 addresses, whether attached to a service or not.

- **Main Route Table**:
  - A VPC automatically comes with a router using a main route table with routing rules (routes).
  - Example: The main route table has a default route with a destination of 10.0.0.0/16 (VPC CIDR block) and a target of local, meaning every resource in the VPC is reachable by every other resource.
  - The default route cannot be deleted.
  - A VPC can be divided into subnets, each a container for routing policies, belonging to one AZ, with a segment of the VPC’s IP address range (e.g., /20 netmask for 4096 IP addresses).
  - AWS reserves the first four and last IP addresses in each subnet CIDR block (e.g., for a 10.0.0.0/20 subnet: 10.0.0.0 for network address, 10.0.0.1 for VPC local router, 10.0.0.2 for DNS resolution, 10.0.0.3 for future use, and 10.0.15.254 for network broadcast).

- **Public Subnets**:
  - A public subnet with an associated internet gateway allows direct access to the internet.
  - Every instance in a public subnet requires a private and a public IP address to gain internet access.
  - An internet gateway is a horizontally scaled, redundant, and highly available VPC component that supports IPv4 and IPv6 traffic, providing a target in VPC route tables for internet-routable traffic and performing NAT for instances with public IPv4 addresses.
  - To make a public subnet reachable, create and attach an internet gateway to the VPC, create an EC2 instance with a public IP address in the subnet, and add a route to the public subnet route table with destination 0.0.0.0/0 and the internet gateway ID as the target.
  - Example: Public subnet route table overrides the main VPC route table to route all traffic (0.0.0.0/0) to the internet gateway.

- **Elastic IP Addresses**:
  - An Elastic IP address is a static, public IPv4 address associated with an instance, transferable to a new instance if the original instance’s health deteriorates.
  - No cost for the first Elastic IP address associated with a running EC2 instance; additional or unattached Elastic IP addresses incur an hourly cost.
  - Elastic IP addresses can also be associated with a load balancer or a VPC network interface.

- **Private Subnets**:
  - A private subnet does not have direct access to the internet and is not reachable from the internet.
  - It’s a best practice to define a custom route table for every subnet containing its own routes.
  - Example: The private subnet route table is identical to the main VPC route table, allowing the EC2 instance to reach any resource in the VPC.
  - A subnet uses the main VPC route table by default if no explicit routing table is associated, and a subnet can only be associated with one route table at a time, though multiple subnets can share a route table.

- **NAT IP Mapping**:
  - When you don’t want to expose a server IP address to other networks or the internet, use a NAT device to replace the private IP of the request initiator with its own public IP.
  - Example sequence:
    - Source server (Private IP: 10.0.0.17) initiates a request to the NAT device with its private IP as the message source IP.
    - NAT device (Public IP: 89.89.0.100) maps the message source IP to its own public IP and forwards the message to the destination server (Public IP: 190.50.20.87).
    - Destination server responds with the destination IP set as 89.89.0.100 to the NAT device.
    - NAT device maps the response message destination IP to the source server’s private IP (10.0.0.17) and forwards the message to the source server.

- **Connecting Private Subnets to the Internet**:
  - Resources in a private subnet may need to connect to the internet (e.g., for downloading patches) while remaining private and not discoverable by the internet.
  - Use a NAT device to allow private subnet resources to communicate with services outside the VPC without receiving unsolicited connection requests.
  - AWS offers two NAT solutions:
    - **NAT Gateway**: An AWS-managed service that incurs an hourly cost, providing better availability and bandwidth with less administrative effort.
    - **NAT Instance**: A user-managed EC2 instance incurring EC2 costs.
  - Example process:
    - EC2 instance in a private subnet initiates a request to a NAT gateway or instance using the private subnet route table (Destination: 0.0.0.0/0, Target: NAT gateway ID).
    - The NAT device replaces the instance’s private IPv4 address with its own address, sends the request to the internet gateway, and receives the response.
    - The NAT device translates the response address back to the instance’s private IPv4 address.
  - AWS recommends NAT gateways over NAT instances for better availability and reduced administration.
  - For resilience in multiple AZs, deploy a NAT gateway in each AZ.
  - For IPv6 private subnets, use an egress-only internet gateway to allow outbound communication over IPv6 while preventing inbound connections from the internet.

- **Activity: Choose the Right Type of Subnet**:
  - Decide whether instances should be placed into a public or private subnet.

- **Recommended Subnet Selections for Each Use Case**:
  - **Database instances**: Private subnet.
  - **Batch-processing instances**: Private subnet.
  - **Web application instances**: Public or private subnet (AWS recommends private subnets behind a load balancer; public subnets required if attaching Elastic IP addresses directly to instances).
  - **NAT gateway or instance**: Public subnet (requires access to an internet gateway).

---

# Section 3: Securing Network Resources
This section outlines how to secure network resources within an Amazon Virtual Private Cloud (VPC) using multiple layers of defense, including security groups, network ACLs, AWS Network Firewall, and bastion hosts. Below, we detail the mechanisms and best practices for protecting your VPC resources.

- **Security Layers of Defense**:
  - Isolating resources in a public subnet is insufficient for securing VPC resources; multiple layers of defense are needed.
  - Network traffic flows from a client application using a secure protocol (e.g., TLS, HTTPS) to an internet gateway, then through the route table layer, network access control list (network ACL) layer, subnet layer, and security group layer to an EC2 instance.
  - Using secure protocols like TLS and HTTPS encrypts data in transit, preventing third-party actors from reading or impersonating traffic.
  - Implement both security groups and network ACLs for defense-in-depth, ensuring a configuration mistake in one does not expose resources to unwanted traffic.

- **Security Groups and Network ACL Scope**:
  - **Security Groups**: Act as a resource firewall (e.g., for EC2 instances), similar to an apartment door allowing only the tenant with a key. They specify rules to filter traffic to and from a resource.
  - **Network ACLs**: Act as a subnet firewall, similar to an apartment doorman allowing only building tenants. They control traffic in and out of subnets.

- **Security Groups**:
  - Stateful firewalls operating at the instance or network interface level, spanning multiple Availability Zones (AZs).
  - Separate inbound and outbound rules; only allow rules can be specified (no deny rules).
  - Example: An inbound rule allows HTTPS TCP traffic from a load balancer security group on port 443.
  - Resources with the same security requirements can be grouped in one security group.
  - Relationships between security groups can be defined (e.g., a database tier security group accepts traffic only from an application tier security group).
  - Stateful: Return traffic is automatically allowed, regardless of rules (e.g., an HTTPS response from an EC2 instance is allowed even if outbound rules restrict HTTPS traffic).
  - Default behavior: No inbound traffic is allowed; all outbound traffic is allowed unless restricted.

- **Network ACLs**:
  - Stateless firewalls at the subnet level with separate inbound and outbound rules that can allow or deny traffic.
  - Each subnet must be associated with one network ACL; the default network ACL allows all IPv4 and IPv6 traffic with a deny-all asterisk (*) rule for misconfigured traffic.
  - Custom network ACLs include a deny-all asterisk rule; numbered rules specify allowed/denied traffic (e.g., inbound rule 100 allows HTTPS TCP traffic from 188.7.55.9/32 on port 443).
  - Stateless: Return traffic must be explicitly allowed by rules.

- **Comparing Security Groups and Network ACLs**:
  - **Security Groups**: Operate at resource level, specify only allow rules, stateful, evaluate all rules, default to no inbound and all outbound traffic.
  - **Network ACLs**: Operate at subnet level, specify allow and deny rules, stateless, evaluate rules in number order (stops at first match), default to allow all inbound and outbound traffic.
  - AWS recommends using security groups over network ACLs for ease of maintenance and flexibility.

- **AWS Network Firewall**:
  - A stateful, managed network firewall and intrusion detection/prevention service for a VPC.
  - Deployed in a firewall subnet to inspect all incoming VPC traffic, adding an additional security layer.
  - Modify VPC route tables to route external traffic through the firewall endpoint (e.g., route table 1 routes all traffic from an internet gateway to the firewall, and route table 2 routes traffic from the firewall to a private subnet EC2 instance).

- **Administrating Resources with Bastion Hosts**:
  - A bastion host in a public subnet provides maintenance access to private subnet resources, minimizing attack risks.
  - Example: A bastion host EC2 instance in a public subnet allows SSH TCP traffic on port 22 from a specified IP address range (Security Group A). The private subnet EC2 instance (Security Group B) allows SSH traffic from Security Group A.
  - Bastion hosts use IAM policies to grant access to VPC resources, ensuring restricted administrative access.

### Key Takeaways: Securing Network Resources
This section summarizes the essential points for securing network resources within an Amazon Virtual Private Cloud (VPC) using multiple layers of defense to protect your network environment effectively.
- Secure AWS infrastructure with multiple layers of defense.
- A security group in a VPC specifies which traffic is allowed to or from AWS resources. It is stateful.
- A network ACL allows or denies specific inbound or outbound traffic at the subnet level. It is stateless.
- Route external VPC traffic through AWS Network Firewall to add an additional layer of traffic security.
- Use a bastion host to administrate private subnet resources from an on-premises environment.

---

# Section 4: Connecting to Managed AWS Services
This section explores how to connect Amazon Virtual Private Cloud (VPC) resources to managed AWS services, such as Amazon S3 and DynamoDB, using VPC endpoints. Below, we detail the mechanisms for establishing secure and efficient connectivity from your VPC to these services.

- **Connecting to Managed AWS Services**:
  - VPC resources, like an EC2 instance in a private subnet, may need to access managed AWS services (e.g., Amazon S3) that operate outside the VPC.
  - Direct access to Amazon S3 via public Region access points routes traffic through the internet, incurring data transfer costs.

- **Interface VPC Endpoints**:
  - Interface VPC endpoints, powered by AWS PrivateLink, enable private connectivity to AWS managed services as if they were within your VPC.
  - Example: An EC2 instance in a private subnet connects to an Amazon S3 bucket via an interface VPC endpoint using an elastic network interface with a private IP address from the subnet.
  - AWS creates an elastic network interface in each specified subnet, representing a virtual network card.
  - Attach an IAM resource policy to the endpoint to control user or application access, securing the endpoint and accessible resources.
  - Interface VPC endpoints incur costs for hourly usage and data processed per month.

- **Setting Up an Interface VPC Endpoint**:
  - Steps in the Amazon VPC console:
    - Specify the AWS service, endpoint service, or AWS Marketplace service to connect to.
    - Choose the VPC and one or more subnets in different Availability Zones (AZs) for resilience against AZ failures.
    - Select a subnet for the interface endpoint; a network interface with a private IP address is created as an entry point for traffic.
    - Specify security groups to control traffic to the network interface; the default VPC security group is used if none is specified.
  - Services cannot initiate requests to VPC resources through the endpoint; only responses to VPC-initiated traffic are returned.

- **Gateway VPC Endpoints**:
  - Gateway VPC endpoints provide direct connectivity to Amazon S3 and DynamoDB using route tables, without AWS PrivateLink.
  - Example: An EC2 instance connects to an Amazon S3 bucket via Gateway VPC endpoint 1, with a route table specifying a prefix list ID as the destination and the endpoint as the target.
  - Similarly, connect to DynamoDB via Gateway VPC endpoint 2 using a prefix list (a group of CIDR blocks).
  - No additional charge for gateway endpoints, and no throughput or packet size limits.
  - Amazon S3 supports both gateway and interface VPC endpoints.

- **Amazon S3 Endpoint Considerations**:
  - **Interface VPC Endpoints**: Use private IP addresses from VPC subnets, allow on-premises and cross-Region access, are billed, offer up to 10 Gbps per AZ (scales to 100 Gbps), and support a maximum packet size of 8500 bytes.
  - **Gateway VPC Endpoints**: Use Amazon S3 public IP addresses, do not allow on-premises or cross-Region access, are not billed, have no bandwidth or packet size limits.
  - Consider: Access method (public vs. private IP), on-premises or cross-Region needs, budget, bandwidth requirements, and maximum packet size.

- **Gateway Load Balancer Endpoint**:
  - Provides private connectivity between security appliances in one VPC (e.g., VPC 2) and an application instance in another VPC (e.g., VPC 1).
  - Example process:
    - Traffic enters VPC 1 via an internet gateway, routes to the Gateway Load Balancer endpoint, then to the Gateway Load Balancer in VPC 2.
    - The Gateway Load Balancer distributes traffic to an EC2 security appliance for inspection, which returns inspected traffic via the same path.
    - Outgoing traffic from the EC2 application instance follows the same route.

- **Key Takeaways: Connecting to Managed AWS Services**:
  - VPC resources can access AWS managed services using VPC endpoints.
  - An interface VPC endpoint uses AWS PrivateLink to access AWS managed services, incurs cost, and has throughput limitations.
  - A gateway VPC endpoint integrates directly with Amazon S3 and DynamoDB, does not incur cost, and has no throughput limitations.
  - Gateway Load Balancer endpoints are used with Gateway Load Balancers to inspect traffic with security appliances.

---

# Section 5: Monitoring Your Network
This section covers how to monitor your network in AWS using tools like Amazon VPC Flow Logs, Reachability Analyzer, Network Access Analyzer, and Traffic Mirroring to troubleshoot and secure your network environment. Below, we outline the key mechanisms for effective network monitoring.

- **Network Troubleshooting Scenarios**:
  - Slow EC2 instance response times may indicate unnecessary or unwanted traffic, such as distributed denial of service (DDoS) attacks overloading resources.
  - Inability to access an EC2 instance via Secure Shell (SSH) may result from security group rules not allowing port 22 traffic, indicating a configuration issue.
  - EC2 database instances in a private subnet not applying patches may require checking security group and route table NAT gateway configurations.
  - Monitoring and automated recovery processes are essential to investigate and resolve such issues.

- **Amazon VPC Flow Logs**:
  - Capture packet-level information about network traffic in your VPC, including all, accepted, or rejected traffic.
  - Apply to the entire VPC, a specific subnet, or an elastic network interface.
  - Deliver logs to Amazon CloudWatch Logs (viewable in the AWS Management Console), Amazon S3 (queryable with Amazon Athena as plain text or Parquet), or Amazon Kinesis Data Firehose (for Amazon OpenSearch Service or third-party solutions like Splunk).
  - Operate outside the VPC network, ensuring no impact on latency or performance.

- **Flow Log IAM Access Policy**:
  - By default, users lack permission to manage flow logs; an IAM role with a policy is needed.
  - Example policy: Allows actions to create, describe, and delete EC2 flow logs on any AWS resource (`"Resource": "*"`).

- **Default Flow Log Record (Part 1)**:
  - Flow log records are log events describing traffic flow within an aggregation interval, using version 2 fields by default.
  - Captured metadata includes:
    - `version`: VPC Flow Logs version (e.g., 2).
    - `account-id`: Network owner AWS account (e.g., 123456789010).
    - `interface-id`: Traffic network interface (e.g., eni-1235b8ca123456789).
    - `srcaddr`: Source IP for incoming traffic or network interface for outgoing (e.g., 172.31.16.139).
    - `dstaddr`: Destination IP for outgoing traffic or network interface for incoming (e.g., 172.31.16.21).
    - `srcport`: Source port (e.g., 20641).
    - `dstport`: Destination port (e.g., 22 for SSH).
    - `protocol`: IANA protocol number (e.g., 6 for TCP).
  - Example: Captures SSH traffic on port 22 using TCP to a network interface in a private subnet from an on-premises network.

- **Default Flow Log Record (Part 2)**:
  - Additional fields include:
    - `packets`: Number of packets transferred (e.g., 20).
    - `bytes`: Number of bytes transferred (e.g., 4249).
    - `start`: Unix time in seconds of first packet received (e.g., 1418530010).
    - `end`: Unix time in seconds of last packet received (e.g., 1418530070).
    - `action`: Indicates if traffic was accepted or rejected (e.g., ACCEPT).
    - `log-status`: Flow log status (e.g., OK for normal logging, NODATA for no traffic, SKIPDATA for skipped records due to capacity or errors).
  - Rejected traffic may occur due to security group/network ACL rules or packets arriving after a connection closes.

- **More VPC Troubleshooting Tools**:
  - **Reachability Analyzer**: Tests connectivity between VPC resources, providing hop-by-hop details if reachable or identifying blocking components (e.g., security group, network ACL, route table, or load balancer) if not.
    - Example: Verify SSH connectivity from an internet gateway to an EC2 instance on port 22 using TCP.
  - **Network Access Analyzer**: Identifies unintended network access to AWS resources, helping verify compliance (e.g., isolating systems processing credit card information).
  - **Traffic Mirroring**: Copies network traffic to security/monitoring appliances for detailed analysis, including message payload, to diagnose performance issues, reverse-engineer attacks, or detect insider abuse/compromised workloads.
    - Example: Mirror inbound TCP/UDP traffic to security appliances for packet inspection.

- **Key Takeaways: Monitoring Your Network**:
  - Use VPC Flow Logs to capture information about the network traffic in your VPC.
  - Flow log records consist of all flows within an aggregation interval.
  - Use Reachability Analyzer to test whether two resources in a VPC have connectivity.
  - Use Network Access Analyzer to identify unintended network access to resources in your AWS account.
  - Use Traffic Mirroring to make a copy of your network traffic to send to security and monitoring appliances.

---

### Module summary
This module prepared you to do the following: 
- Explain the role of a virtual private cloud (VPC) in Amazon Web Services (AWS) Cloud networking.
- Identify the components in a VPC that can connect an AWS networking environment to the internet.
- Isolate and secure resources within your AWS networking environment.
- Create and monitor a VPC with subnets, an internet gateway, route tables, and a security group.
- Use the AWS Well-Architected Framework principles when creating and planning a network environment.
