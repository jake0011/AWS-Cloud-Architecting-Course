# AWS Academy Cloud Architecting 3 – Capstone Project

## Overview and Objectives

Throughout this course, you have completed hands-on labs. You have used different AWS services and features to create compute instances, install operating systems (OSs) and software, deploy code, and secure resources. You practiced how to enable load balancing and automatic scaling and how to architect for high availability to build lab-specific applications.

In this project, you’re challenged to use familiar AWS services to build a solution without step-by-step guidance. Specific sections of the assignment are meant to challenge you on skills that you have acquired throughout the Academy Cloud Architecting (ACA) course.

By the end of this project, you should be able to apply the architectural design principles that you learned in this course to do the following:

- Create a database (DB) instance that the PHP application can query.
- Create and deploy the highly available PHP application with the load distributed across multiple web servers and Availability Zones.
- Use AWS Secrets Manager.
- Import data into a MySQL database from a SQL dump file.
- Secure the application to prevent public access to application servers and backend systems.

## The Lab Environment and Monitoring Your Budget

This environment is long lived. When the session timer gets to 0:00, the session will end, but any data and resources that you created in the Amazon Web Services (AWS) account will be retained. If you later launch a new session (for example, the next day), you will find that your work is still in the lab environment. Also, at any point before the session timer reaches 0:00, you can choose Start Lab again to extend the lab session time.

**Important:** Monitor your lab budget in the lab interface. When you have an active lab session, the latest known remaining budget information displays at the top of this screen. This data comes from AWS Budgets, which typically updates every 8–12 hours. Therefore, the remaining budget that you see might not reflect your most recent account activity. If you exceed your lab budget, your lab account will be disabled, and all progress and resources will be lost. Therefore, it’s important for you to manage your spending.

The following are a few suggestions to avoid overspending:

- Launch only the number of EC2 instances that you need, sized to your requirements.
- Delete resources and data that you no longer need.
- Typically, it is compute-type resources, such as Amazon Elastic Compute Cloud (Amazon EC2) instances, Amazon Relational Database Service (Amazon RDS) database instances, AWS Cloud9, and NAT gateway instances, that you leave running that most quickly use up your budget. Turn these off when you no longer need them, or–better yet–delete them.
- Stop any NAT gateways, EC2 instances, and Amazon RDS instances when they are not in use.
- Be aware that if you do stop an Amazon RDS instance or cluster and leave it stopped for 7 days, AWS might start it again automatically, which will increase the cost impact.
- NAT gateway resources, if left running, will also use up your budget.

To estimate your costs, use the AWS Pricing Calculator.

Here is a sample that shows the cost to run the resources in the us-east-1 Region for a month and an entire year:

*[Cost estimate diagram placeholder]*

**Note:** Pricing is subject to change. The calculation in the previous sample is an example from a point in time.

If you want to reset your AWS account to the original settings before you ran sessions of this lab environment, choose **Reset**. **CAUTION:** If you choose Reset and then choose Yes to confirm that you want to reset, you will permanently delete everything that you have created or stored in the AWS account.

## AWS Service Restrictions

In this lab environment, access to AWS services and service actions might be restricted to the ones that are needed to complete the lab instructions. You might encounter errors if you attempt to access other services or perform actions beyond the ones that are described in this lab.

## Scenario

Example Social Research Organization is a (fictitious) nonprofit organization that provides a website for social science researchers to obtain global development statistics. For example, visitors to the site can look up various data points, such as the life expectancy for any country in the world over the past 10 years.

Shirley Rodriguez, a researcher at the nonprofit organization, developed the website. She thought it would be valuable to share the data that she had gathered with other researchers. Shirley stores the data in a MySQL database, and the data is available through a PHP website that she built. She initially published the site through a commercial hosting company that provides limited support for technical issues and security.

Over the past year, Shirley’s website has grown in popularity. As a result of increased traffic, she started receiving complaints that the site is not as responsive as it used to be. She also experienced an attempted ransomware security breach. The security breach was unsuccessful, but her supervisor, Mateo Jackson, suggested that Shirley investigate new ways to host the website.

Shirley heard about AWS and initially moved her website and database to an EC2 instance that runs in a public subnet. She also runs an instance of MySQL on the same EC2 instance.

Shirley approached your team to make sure that her current design follows architectural best practices. She wants to make sure that she has a robust and secure website. One of your colleagues started the process of migrating the site to a more secure implementation, but they were reassigned to another project. Your tasks are to complete the implementation, make sure that the website is secure, and confirm that the website returns data from the query page.

The following summary lists the solution requirements and provides a diagram of the current environment.

### Solution Requirements

- Provide secure hosting of the MySQL database.
- Provide secure access to the database.
- Provide anonymous access to web users.
- Run the website on a t2.micro EC2 instance running in private subnets and provide Secure Shell (SSH) access to administrators.
- Provide high availability to the website through a load balancer.
- Provide automatic scaling that uses a launch template.

As part of the lab provisioning, the components in the following architecture diagram are already available to you in the lab:

*[Starting architecture diagram placeholder]*

Following architecture diagram shows the infrastructure components available at the start of the lab.

### Assumptions

This project will be built in a controlled lab environment that has restrictions on services, features, and budget. Consider the following assumptions for the project:

- The application is deployed in one AWS Region (the solution does not need to be multi-Regional).
- The website does not need to be available over HTTPS or a custom domain.
- The solution is deployed on Amazon Linux 2023 machines by using the PHP code that is provided. A template is already available in the lab to launch EC2 instances.
- The solution uses the PHP code as provided unless the instructions specifically direct you to change the code.
- The solution uses services and features within the restrictions of the lab environment.
- The solution uses all the three pairs of subnets provisioned.
- The solution uses the available AWS Identity and Access Management (IAM) role.
- The database can be hosted in a single Availability Zone or multiple Availability Zones.
- Store database connection information in Secrets Manager.
- The website is publicly accessible without authentication.

**Disclaimer:** A security best practice is to allow access to the website through authentication. However, because you are building this application as a part of the course, those features are beyond the scope of this project.

## Project Deliverables

To complete this assignment, you must do the following:

- Submit a diagram that illustrates your solution.
- Deploy and demonstrate a PHP application that meets the system requirements outlined in these lab instructions.
- Submit a written summary of the design decisions that you made to achieve the result.

## Approach

**Recommendation:** Develop your project solution that is mentioned in the following steps. This will help you map the building blocks with the solution requirements.

### Access the AWS Management Console

At the top of these instructions, choose **Start Lab** to start the lab session.

The lab session starts.

A timer displays at the top of the page and shows the time remaining in the session.

**Tip:** To refresh the session length at any time, choose **Start Lab** again before the timer reaches 0:00.

Before you continue, wait until the circle icon to the right of the **AWS** link in the upper-left corner turns green.

To connect to the AWS Management Console, choose the **AWS** link in the upper-left corner above the terminal window.

A new browser tab opens and connects you to the console.

**Tip:** If a new browser tab does not open, a banner or icon is usually at the top of your browser with a message that your browser is preventing the site from opening pop-up windows. Choose the banner or icon, and then choose **Allow pop-ups**.

### Step 1: Creating an Amazon RDS MySQL Database

Create an Amazon RDS MySQL database.

**Tips:**

- The database security group should allow only the web application to access the database.
- Use Secrets Manager to store the username and password.
- Keep the initial database as countries.
- Use the database subnet group already provisioned in the lab.

**Note:** Turning on additional features while configuring the database might incur charges that impact your cost estimate. Turn on only the features that are essential to your work.

**References:**

- AWS Academy Cloud Architecting – Lab: Guided Lab – Creating an Amazon RDS Database
- Create and Connect to a MySQL Database with Amazon RDS

### Step 2: Creating an Application Load Balancer

Launch an Application Load Balancer in the available public subnets. The load balancer's DNS name will be used to access your web application.

**Tip:** Use a minimum of two Availability Zones.

**References:**

- Getting Started with Elastic Load Balancing
- AWS Academy Cloud Architecting – Lab: Creating a Highly Available Environment

### Step 3: Creating the Application Server in an Auto Scaling Group

Create a launch template that you will use to launch EC2 instances in an Auto Scaling group.

Use an Auto Scaling group to launch the EC2 instances (application servers) that host the web application.

To install the required web application on the virtual machine, use the PHP code that is available.

Use the Application Load Balancer that you created earlier.

**Note:** An EC2 launch template named Example-LT is provided in the AWS account. It already has both the SQL dump file and the unzipped PHP application resources built into it.

**Tips:**

- Use a target tracking policy.
- Set the Auto Scaling group size according to your estimated requirements.
- You can use the default values (for example, for group size and CPU utilization) initially and then adjust them later as needed.

**Reference:**

- AWS Academy Cloud Architecting – Lab: Creating a Highly Available Environment

### Step 4: Importing Data into an RDS MySQL Database

Migrate the data from the original database, which is on an EC2 instance, to the new Amazon RDS database. Use the data dump file that is available.

Refer to Secrets Manager for the username and password.

Use the same database that you created while provisioning an RDS MySQL database.

**Reference:**

For commands, see "Importing Data to an Amazon RDS MariaDB or MySQL Database with Reduced Downtime."

### Step 5: Testing the Application and Reviewing Imported Data

To test the application, access the web application from a browser by using the load balancer URL.

To review and query the imported data, use the query functionality in the application.

---

## Additional Step-by-Step Guidance

The lab is a capstone project designed to test your skills without providing detailed steps, so below I'll provide supplemental step-by-step instructions for each major step. These are based on standard AWS best practices and the references mentioned. I'll assume you're working in the us-east-1 Region (as per the cost estimate example) and using the pre-provisioned resources like subnets, IAM roles, and the Example-LT launch template. Always monitor your budget as emphasized.

### Preliminary: Accessing the Console and Verifying Pre-Provisioned Resources

1. Start the lab session as described.
2. Once in the AWS Management Console, navigate to **VPC** > **Subnets** to confirm the three pairs of subnets (public and private across Availability Zones).
3. Go to **EC2** > **Launch Templates** and verify the "Example-LT" template exists (it includes the PHP app and SQL dump).
4. Check **IAM** > **Roles** for the available IAM role (likely for EC2 instances to access Secrets Manager).
5. Navigate to **RDS** > **Subnet Groups** to confirm the pre-provisioned DB subnet group.

### Step 1: Creating an Amazon RDS MySQL Database (Detailed Guide)

1. In the AWS Console, search for **RDS** and select **Databases** > **Create database**.
2. Choose **Standard create** method.
3. Engine: **MySQL**.
4. Edition: Community Edition (free tier eligible if available).
5. Version: Latest compatible (e.g., 8.0.x).
6. Templates: **Free tier** or **Dev/Test** to minimize costs.
7. DB instance identifier: Something like "countries-db".
8. Master username: Choose a secure one (e.g., "admin").
9. Master password: Generate a strong password (note it down temporarily).
10. DB instance class: db.t3.micro (or t2.micro if available) to keep costs low.
11. Storage: General Purpose SSD, 20 GiB (minimum).
12. Availability & durability: For high availability, select **Multi-AZ deployment** (but single-AZ is allowed per assumptions to save costs; choose based on needs).
13. Connectivity:
    - VPC: The default or lab-provided VPC.
    - Subnet group: Use the pre-provisioned one (e.g., "default" or lab-specific).
    - Public access: **No** (for security).
    - VPC security group: Create a new one (e.g., "db-sg"). Later, edit it to allow inbound TCP 3306 only from the web app's security group (you'll create this in Step 3).
14. Database authentication: Password authentication.
15. Additional configuration:
    - Initial database name: "countries".
    - Backup: Enable automated backups (short retention to minimize costs).
    - Monitoring: Disable enhanced monitoring if not needed.
    - Performance Insights: Disable.
    - Deletion protection: Enable if desired.
16. Create the database. Wait for it to become available (5-10 minutes).
17. Store credentials in Secrets Manager:
    - Search for **Secrets Manager** > **Store a new secret**.
    - Secret type: **Credentials for Amazon RDS database**.
    - Username: Your master username.
    - Password: Your master password.
    - Database: Select your new RDS instance.
    - Secret name: Something like "countries-db-credentials".
    - Create the secret.
18. Update the DB security group: In RDS > Databases > Your DB > Connectivity & security > Security groups. Edit inbound rules to allow 3306 from the future web app SG (placeholder for now; update after Step 3).

**Security Note:** Ensure no public access; only app servers can connect.

### Step 2: Creating an Application Load Balancer (Detailed Guide)

1. In the Console, search for **EC2** > **Load Balancers** > **Create load balancer**.
2. Choose **Application Load Balancer**.
3. Name: "app-lb".
4. Scheme: Internet-facing.
5. IP address type: IPv4.
6. VPC: Lab VPC.
7. Mappings: Select at least two Availability Zones and their public subnets (use the pre-provisioned public subnets).
8. Security groups: Create a new SG (e.g., "lb-sg") allowing inbound HTTP (80) from 0.0.0.0/0 (for public access).
9. Listeners: HTTP on port 80, forward to a new target group.
10. Target group:
    - Name: "app-tg".
    - Target type: Instances.
    - Protocol: HTTP, Port: 80.
    - Health checks: HTTP, path "/health" (or default "/"; adjust based on PHP app).
11. Create the ALB. Note the DNS name (e.g., app-lb-123456789.us-east-1.elb.amazonaws.com) for testing later.

**Tip:** For high availability, ensure it spans multiple AZs.

### Step 3: Creating the Application Server in an Auto Scaling Group (Detailed Guide)

1. Use the provided "Example-LT" launch template (it has PHP code and SQL dump pre-installed).
2. If needed, create or edit a launch template:
    - EC2 > Launch Templates > Create or copy "Example-LT".
    - AMI: Amazon Linux 2023.
    - Instance type: t2.micro.
    - Key pair: Create or use existing for SSH.
    - Network: Private subnets across AZs.
    - Security group: Create "app-sg" allowing inbound 80 from lb-sg, 22 (SSH) from your IP or bastion, and 3306 outbound to db-sg.
    - IAM instance profile: Attach the lab IAM role (for Secrets Manager access).
    - User data: Add script to install PHP, Apache, etc., if not in template. Example:
       ```
       #!/bin/bash
       yum update -y
       yum install -y httpd php php-mysqlnd
       systemctl start httpd
       systemctl enable httpd
       # Copy PHP files from template to /var/www/html/
       # Configure PHP to use Secrets Manager for DB creds (edit code to fetch from Secrets Manager)
       ```
    - Note: Update PHP code to fetch DB creds from Secrets Manager using AWS SDK (e.g., in config.php: use AWS PHP SDK to get secret).
3. Create Auto Scaling Group:
    - EC2 > Auto Scaling Groups > Create.
    - Name: "app-asg".
    - Launch template: "Example-LT".
    - VPC: Lab VPC.
    - Subnets: Private subnets across all three AZ pairs.
    - Load balancing: Attach to existing target group "app-tg".
    - Group size: Min 1, Desired 2, Max 3 (adjust for budget).
    - Scaling policies: Target tracking, metric CPUUtilization, target 50%.
    - Health checks: ELB health checks.
4. Create. Instances will launch in private subnets.
5. Update DB SG: Allow 3306 from "app-sg".
6. SSH to instances via bastion (if needed) to verify PHP app is running and connecting to RDS (using Secrets Manager).

**Note:** Ensure app servers are in private subnets, no public IP.

### Step 4: Importing Data into an RDS MySQL Database (Detailed Guide)

1. The SQL dump is in the launch template (likely at /home/ec2-user/countries.sql or similar).
2. To import: Use an EC2 instance (e.g., one from ASG or a temporary bastion).
3. SSH to an app instance.
4. Install MySQL client: `yum install -y mysql`.
5. Fetch creds from Secrets Manager:
   - Ensure instance has IAM role with SecretsManagerReadWrite policy.
   - Use AWS CLI: `aws secretsmanager get-secret-value --secret-id countries-db-credentials --query SecretString --output text > creds.json`.
   - Parse JSON for username/password/host (RDS endpoint).
6. Import: `mysql -h <rds-endpoint> -u <username> -p<password> countries < /path/to/countries.sql`.
7. Verify: Connect with `mysql` and run `SHOW TABLES;` in countries DB.

**Reference Adjustment:** Use reduced downtime method if needed, but for small dump, direct import works.

### Step 5: Testing the Application and Reviewing Imported Data (Detailed Guide)

1. Get ALB DNS from EC2 > Load Balancers.
2. Open in browser: http://<alb-dns>/ (should show PHP app).
3. Test query: Use app's query page to fetch data (e.g., life expectancy for a country).
4. If issues: Check CloudWatch logs, instance health, DB connection.
5. For deliverables:
   - Diagram: Use AWS Architecture Icons or draw.io to create a final architecture (VPC, ALB in public, ASG in private, RDS in private, Secrets Manager).
   - Summary: Write decisions like "Chose Multi-AZ RDS for HA, private subnets for security, ASG for scaling."
6. Clean up: Stop/delete resources to save budget.

If you encounter errors or need specifics (e.g., exact PHP code changes), provide more details from your lab session!
