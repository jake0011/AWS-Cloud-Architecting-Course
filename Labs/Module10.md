
# Guided Lab: Creating a Highly Available Environment

---

## Lab overview and objectives

Critical business systems should be deployed as highly available applications; that is, applications remain operational even when some components fail. To achieve high availability in Amazon Web Services (AWS), you run services across multiple Availability Zones.

Many AWS services are inherently highly available, such as load balancers. Many AWS services can also be configured for high availability, such as deploying Amazon Elastic Compute Cloud (Amazon EC2) instances in multiple Availability Zones.

In this lab, you start with an application that runs on a single EC2 instance and then make it highly available.

After completing this lab, you should be able to do the following:

- Inspect a provided virtual private cloud (VPC).
- Create an Application Load Balancer.
- Create an Auto Scaling group.
- Test the application for high availability.

At the end of this lab, your architecture will look like the following example:

> Final lab Architecture showing all the components used to build a highly available architecture.

---

## Duration

The lab requires approximately **40 minutes** to complete.

---

## AWS service restrictions

In this lab environment, access to AWS services and service actions might be restricted to the ones that are needed to complete the lab instructions. You might encounter errors if you attempt to access other services or perform actions beyond the ones that are described in this lab.

---

## Accessing the AWS Management Console

1. At the top of these instructions, choose **Start Lab**.
   - The lab session starts.
   - A timer displays at the top of the page and shows the time remaining in the session.
   - **Tip:** To refresh the session length at any time, choose *Start Lab* again before the timer reaches 0:00.
2. Wait until the circle icon to the right of the AWS link in the upper-left corner turns green.
3. When the lab environment is ready, the AWS Details panel displays.
4. To connect to the AWS Management Console, choose the **AWS link** in the upper-left corner, above the terminal window.
   - A new browser tab opens and connects you to the console.
   - **Tip:** If a new browser tab does not open, your browser may be blocking pop-ups. Allow pop-ups for the lab site.
5. Arrange the AWS Management Console so that it appears alongside these instructions.

> **Do not change the Region unless specifically instructed to do so.**

---

## Task 1: Inspecting your VPC

This lab begins with an environment already deployed through AWS CloudFormation. This environment includes the following:

- A VPC  
- Public and private subnets in two Availability Zones  
- An internet gateway associated with the public subnets  
- A NAT gateway in one of the public subnets  
- An Amazon RDS instance in one of the private subnets

> Diagram shows the starting architecture of the lab where you inspect the existing VPC as part of Task 1.

### Steps

1. On the AWS Management Console, search for and open **VPC**.
2. In the left navigation pane, from the *Filter by VPC* dropdown list, choose **Lab VPC**.
3. In the left navigation pane, choose **Your VPCs**.
   - Select **Lab VPC**.
   - In the *Details* tab, note that the IPv4 CIDR is `10.0.0.0/16`.
4. In the left navigation pane, choose **Subnets**.
   - Observe **Public Subnet 1**:
     - VPC: *Lab VPC*
     - IPv4 CIDR: `10.0.0.0/24`
     - Availability Zone: *(varies)*
   - Select **Public Subnet 1**, and view:
     - **Route table** tab:
       - Local route for `10.0.0.0/16`.
       - Internet route for `0.0.0.0/0` → internet gateway.
     - **Network ACL** tab:
       - Default rules (permit all traffic).
5. In the left navigation pane, choose **Internet gateways**.
   - Confirm that **Lab IG** is attached to **Lab VPC**.
6. In the left navigation pane, choose **Security groups**.
   - Select **Inventory-DB**.
   - Inbound rules: MySQL/Aurora (port 3306) from `10.0.0.0/16`.
   - Outbound rules: All traffic allowed by default.

---

## Task 2: Creating an Application Load Balancer

To build a highly available application, it is a best practice to launch resources in multiple Availability Zones.

Because the application runs on multiple servers, you need a way to distribute traffic — achieved through a **load balancer**. The load balancer also performs **health checks** on instances.

> Diagram shows the architecture with additional components added in this task.

### Steps

1. On the AWS Management Console, search for and open **EC2**.
2. In the left navigation pane, choose **Load Balancers**, then **Create load balancer**.
3. Under **Application Load Balancer**, choose **Create**.
4. **Basic configuration:**
   - Load balancer name: `Inventory-LB`
5. **Network mapping:**
   - VPC: `Lab VPC`
   - Mappings: Choose *Public Subnet 1* and *Public Subnet 2*.
6. **Security groups:**
   - Choose **Create a new security group** (opens new tab).
   - Security group name: `Inventory-LB`
   - Description: `Enable web access to load balancer`
   - VPC: `Lab VPC`
   - Inbound rules:
     - Type: HTTP | Source: Anywhere-IPv4
     - Type: HTTPS | Source: Anywhere-IPv4
   - Choose **Create security group**.
7. Return to the load balancer setup tab.
   - Refresh Security groups.
   - Select `Inventory-LB`.
   - Remove the default security group.
8. **Listeners and routing:**
   - Choose **Create target group** (opens new tab).

### Target Group Configuration

- Target type: *Instances*  
- Target group name: `Inventory-App`  
- VPC: *Lab VPC*  
- **Health checks:**
  - Healthy threshold: `2`
  - Interval: `10 seconds`

Skip target registration for now and **Create target group**.

Return to the load balancer tab, refresh, choose **Inventory-App** as the default action, and **Create load balancer**.

Choose **View load balancer**.

---

## Task 3: Creating an Auto Scaling group

Amazon EC2 Auto Scaling automatically launches or terminates instances based on policies and health checks. It distributes instances across Availability Zones to maintain high availability.

> Diagram shows architecture with additional components added in this task.

---

### Task 3.1: Creating an AMI for Auto Scaling

1. Open **EC2**, then choose **Instances**.
2. Wait until **Web Server 1** shows *2/2 checks passed*.
3. Select **Web Server 1**, then:
   - **Actions → Image and templates → Create image**
   - Image name: `Web Server AMI`
   - Description: `Lab AMI for Web Server`
   - Choose **Create image**
4. Note the AMI ID displayed.

---

### Task 3.2: Creating a launch template and Auto Scaling group

#### Create Launch Template

1. In the left navigation pane, choose **Launch Templates → Create launch template**.
2. Configure:
   - Launch template name: `Inventory-LT`
   - Auto Scaling guidance: *Provide guidance*
   - AMI: *Web Server AMI*
   - Instance type: `t2.micro`
   - Key pair: `vockey`
   - Security group: `Inventory-App`
   - IAM instance profile: `Inventory-App-Role`
   - Enable *Detailed CloudWatch monitoring*.
   - **User data:**

```bash
#!/bin/bash
# Install Apache Web Server and PHP
yum install -y httpd mysql
amazon-linux-extras install -y php7.2
# Download Lab files
wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-200-ACACAD-3-113230/12-lab-mod10-guided-Scaling/s3/scripts/inventory-app.zip
unzip inventory-app.zip -d /var/www/html/
# Download and install the AWS SDK for PHP
wget https://github.com/aws/aws-sdk-php/releases/download/3.62.3/aws.zip
unzip aws -d /var/www/html
# Turn on web server
chkconfig httpd on
service httpd start
````

Choose **Create launch template**.

---

#### Create Auto Scaling Group

1. After success message, open `Inventory-LT`.
2. **Actions → Create Auto Scaling group**
3. **Step 1:**

   * Name: `Inventory-ASG`
   * Launch template: `Inventory-LT`
4. **Step 2:**

   * VPC: `Lab VPC`
   * Subnets: *Private Subnet 1* and *Private Subnet 2*
5. **Step 3:**

   * Attach to existing load balancer → `Inventory-App`
   * Turn on ELB health checks
   * Health check grace period: `90 seconds`
   * Enable group metrics collection (1-minute interval)
6. **Step 4:**

   * Desired capacity: `2`
   * Min: `2`
   * Max: `2`
   * No scaling policies (static capacity)
7. **Step 5:** Skip.
8. **Step 6:** Add tag:

   * Key: `Name`
   * Value: `Inventory-App`
9. **Step 7:** Review and **Create Auto Scaling group**.

> Initially, 0 instances show but Auto Scaling launches 2 instances.
> After a minute, refresh to see both running.

---

## Task 4: Updating security groups

The application you deployed is a **three-tier architecture**. You now configure the security groups to enforce these tiers.

> Following images show various security layers...

```

---

Would you like me to include **Task 4’s detailed steps** too (they seem to be missing from your text)? I can continue formatting that section consistently if you have the rest.
```
