### Question 1

A solutions architect wants to propose a microservice architecture on AWS to management. Which serverless computing benefits should they include in the proposal?
(Select **THREE**.)

* [ ] Predictable performance
* [x] **Built-in high availability**
* [x] **Continuous scaling**
* [ ] Full control over runtime environment
* [x] **Pay-for-value services**
* [ ] Lessens amount of server maintenance

**Rationale:**
Serverless computing on AWS (e.g., AWS Lambda, API Gateway, DynamoDB) provides **automatic high availability**, **continuous and elastic scaling**, and a **pay-for-value pricing model** where you only pay for the compute time you use. It abstracts server management, but does not offer predictable performance or full control over the runtime environment.

---
### Question 2

A developer needs to create and deploy a simple web form to capture employee commute information for the company internal employee website. Which is a **serverless solution** for creating the simple web form?

* [ ] Host static assets in an Amazon S3 bucket, use an Amazon RDS database, and use Amazon API Gateway and AWS Lambda functions to interact with the database.
* [x] **Host static assets in an Amazon S3 bucket, use an Amazon DynamoDB table, and use Amazon API Gateway and AWS Lambda functions to interact with the database.**
* [ ] Host website assets in a container in an Amazon Elastic Container Service (Amazon ECS) cluster with EC2 instance nodes, use an Amazon DynamoDB table, and use server-side scripts to interact with the database.
* [ ] Host website assets in a container in an Amazon Elastic Container Service (Amazon ECS) cluster with Fargate nodes, use an Amazon RDS database, and use server-side scripts to interact with the database.

**Rationale:**
A fully **serverless web application** typically combines **Amazon S3** for static hosting, **Amazon API Gateway** and **AWS Lambda** for business logic, and **Amazon DynamoDB** for data storage. This eliminates server provisioning and maintenance while ensuring scalability, cost efficiency, and high availability.

---
### Question 3

Why would a solutions architect recommend using a **microservice architecture**? (Select TWO.)

* [ ] Industry-standard interfaces
* [ ] **HTTP(S) communication**
* [ ] Open-source code
* [x] **Solves a specific business problem**
* [x] **Independent from other components**

**Rationale:**
A **microservice architecture** structures an application as a collection of **independent, loosely coupled services**, each designed to handle a **specific business function**. These services often communicate over **lightweight protocols like HTTP(S)**, allowing teams to develop, deploy, and scale components independently for improved agility and resilience.

---
### Question 4

Which scaling configuration does a team lead need to apply to accomplish **Lambda function scaling**?

* [ ] Configure the auto scaling parameter in the function configuration.
* [ ] Provision enough function instances to meet the maximum predicted load.
* [x] **None because the AWS Lambda service scales functions automatically.**
* [ ] Launch functions in Auto Scaling groups.

**Rationale:**
**AWS Lambda** automatically scales functions in response to incoming requests. Each event triggers an instance of the function, allowing it to scale seamlessly without manual configuration or provisioning. This eliminates the need for traditional Auto Scaling mechanisms or pre-provisioned instances.

---
### Question 5

A solutions architect has to use a custom library in an architecture that uses **Lambda functions** as the compute option. Which Lambda feature should be used for optimal function deployments?

* [ ] Lambda destinations
* [ ] Lambda function URLs
* [x] **Lambda layers**
* [ ] Lambda triggers

**Rationale:**
**Lambda Layers** allow you to package and share custom libraries, dependencies, and runtime code across multiple Lambda functions. This promotes cleaner deployments, smaller function packages, and easier updates—ideal for maintaining consistent environments when using shared or custom libraries.

---
### Question 6

Which set of statements is true for software packaged as a **container**?

* [ ] Standardized, portable application code packages that contain code and code dependencies. A container is run by a container engine. A container includes a guest operating system.
* [ ] Portable application code packages that contain code and code dependencies. A container is run by a hypervisor. A container does not include a guest operating system.
* [ ] Portable application code packages that contain code and code dependencies. A container is run by a hypervisor. A container includes a guest operating system.
* [x] **Standardized, portable application code packages that contain code and code dependencies. A container is run by a container engine. A container does not include a guest operating system.**

**Rationale:**
Containers package **application code and dependencies** together, providing a **lightweight and portable** runtime environment. They share the host OS kernel (no guest OS), making them faster and more efficient than virtual machines. Containers are managed by a **container engine** such as Docker or containerd.

---
### Question 7

Which is the most effective container deployment using **Amazon Elastic Container Service (Amazon ECS)** containers when refactoring a monolithic application to use a **microservice architecture**?

* [ ] Refactor the application and centralize common functions to create a smaller code footprint.
* [ ] Port the application to a new image, and run it in a container that Amazon ECS manages.
* [ ] Create services that each provide a distinct function of the application, and run multiple services in a single container that Amazon ECS manages.
* [x] **Create services that each provide a distinct function of the application, and run each service in a separate container that Amazon ECS manages.**

**Rationale:**
Microservice architecture promotes **service isolation and independent scalability**. Each service should have its own container, allowing ECS to manage **deployment, scaling, and recovery** independently. Running multiple services in a single container breaks this isolation, defeating the purpose of microservices.

---
### Question 8

An environmental science organization wants to provide **HTTPS read-only access** to its **sensor data stored in Amazon DynamoDB**. The user usage pattern is **intermittent with 68% idle time per month**. Which solution is the most cost-efficient and secure with the least amount of operational maintenance?

* [ ] Create user accounts in the organization's systems to allow direct access to the DynamoDB sensor database.
* [x] **Create a public interface by using Amazon API Gateway with Lambda functions accessing the DynamoDB sensor database.**
* [ ] Create a microservices architecture by using Amazon Elastic Container Service (Amazon ECS) connecting to the DynamoDB sensor database.
* [ ] Create web proxy servers on Amazon EC2 instances in an Auto Scaling group connecting to the DynamoDB sensor database, which is served by an Elastic Load Balancing load balancer.

**Rationale:**
Using **Amazon API Gateway** with **AWS Lambda** provides a **serverless, HTTPS-enabled**, and **cost-efficient** interface for on-demand access to DynamoDB. The organization pays only for requests during active use (no charges during idle time). This approach minimizes maintenance since there are **no servers or scaling infrastructure to manage**, while maintaining **strong IAM-based access control and encryption (HTTPS)** for secure read-only data delivery.

---
### Question 9

Which workflows are suitable to implement with **AWS Step Functions** when the goal is to reduce **operational overhead**? *(Select THREE.)*

* [ ] Send messages to multiple Amazon EC2 instances when a file is created or modified in an Amazon S3 bucket
* [x] **Deploy different kinds of infrastructure that are based on environment variables**
* [x] **Coordinate multi-step analytics and machine learning workflows**
* [x] **Update inventory and run a shipment workflow when a customer purchases an item on an e-commerce site**
* [x] **Implement manual approval in a security incident response workflow**
* [ ] Send a group of email addresses when the costs for an AWS account exceed a threshold

**Rationale:**
**AWS Step Functions** is ideal for **orchestrating complex, multi-step workflows** that require coordination between multiple AWS services without manual intervention or custom orchestration code. It reduces operational overhead by automating:

* **Infrastructure deployment** workflows using environment variables,
* **Analytics and ML pipelines** involving data preprocessing and model training,
* **Transactional workflows** such as order fulfillment or inventory updates, and
* **Human-in-the-loop workflows**, such as approvals in security responses.

Tasks like direct EC2 messaging or billing alerts are better handled by **Amazon SNS, CloudWatch Alarms, or Lambda**, not Step Functions.

---
### Question 10

A developer wants to implement a manual approval step using an **AWS Step Functions** state machine. The manual approval state must **pause until an approval or rejection notification** is received from an approver’s email. Which should be implemented in the Step Functions state machine?

* [ ] A map state with an activated wait for callback parameter
* [ ] A map state with automated callback parameters
* [ ] A task or activity state with automated callback parameters
* [x] **A task or activity state with an activated wait for callback parameter**

**Rationale:**
To handle **manual approval workflows**, Step Functions supports **callback patterns**.
A **task or activity state with “waitForTaskToken”** pauses execution until an external process (such as an email approval Lambda function or API call) sends a **Task Token** back using the `SendTaskSuccess` or `SendTaskFailure` API.
This approach allows the workflow to **pause safely and resume only when the approval or rejection is received**, making it the best fit for human-in-the-loop or manual approval scenarios.
