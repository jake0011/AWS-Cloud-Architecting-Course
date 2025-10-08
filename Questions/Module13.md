### Question 1

Which statement describes the difference between tightly and loosely coupled architectures?

* [ ] Loose coupling increases the complexity of scaling. Tight coupling decreases it.
* [ ] Loosely coupled architectures must use managed services, and tightly coupled architectures don't have this limitation.
* [ ] Tightly coupled architectures are more likely to fail than loosely coupled architectures.
* [x] **Components in a tightly coupled architecture are highly dependent on each other. In a loosely coupled architecture, components aren't highly dependent on each other.**

**Rationale:**
In a **tightly coupled architecture**, system components are **interdependent**, meaning a change or failure in one component can directly affect others. In contrast, a **loosely coupled architecture** separates components through well-defined interfaces or message queues, reducing dependencies and improving **fault tolerance**, **scalability**, and **maintainability**.

---

### Question 2

Which statements describe Amazon Simple Queue Service (Amazon SQS)? (Select **THREE**)

* [x] **Stores and optionally encrypts messages until they are processed and deleted**
* [ ] Supports email and text messaging
* [x] **Requires a consumer to poll the queue for messages**
* [ ] Supports standard queues and topics
* [ ] Sends push notifications to consumers
* [x] **Enables you to decouple and scale microservices, distributed systems, and serverless applications**

**Rationale:**
**Amazon SQS** is a fully managed message queuing service that **stores messages securely** until processed. Consumers **poll** queues to retrieve messages (it does not push them). SQS supports **Standard** and **FIFO** queues—not topics (those belong to SNS). It’s widely used to **decouple components** in distributed systems and **scale microservices** efficiently.

---

### Question 3

Which statements are true when using an Amazon Simple Queue Service (Amazon SQS) **standard queue**? (Select **TWO**)

* [x] **A message might be delivered more than once.**
* [ ] Messages must be processed in the order that they are sent.
* [ ] A message must be processed only once.
* [x] **Messages can be sent in any order.**
* [ ] Messages can be assigned a priority.

**Rationale:**
**Amazon SQS Standard Queues** provide **at-least-once delivery**, meaning a message **might be delivered more than once**. They also offer **best-effort ordering**, so messages can arrive **out of order**. For strict ordering and exactly-once processing, **FIFO queues** are required instead.

---

### Question 4

A fleet of Amazon EC2 instances process videos that users upload. Which function can Amazon Simple Queue Service (Amazon SQS) perform in this application?

* [ ] The application writes the video files to an SQS queue. EC2 instances with available processing capacity pull the next video from the queue.
* [ ] An SQS queue receives messages from the application and notifies all available EC2 instances that videos are available.
* [x] **The application places job messages in an SQS queue. EC2 instances with available processing capacity pull the next job message from the queue.**
* [ ] EC2 instances put edited video files in an SQS queue. The application retrieves the videos from the queue.

**Rationale:**
**Amazon SQS** is a message queuing service used to **decouple application components**. In this case, the application sends **job messages** (not actual video files) to an SQS queue, and **EC2 instances poll the queue** for available jobs to process. This design increases scalability and resilience by allowing the processing layer to work asynchronously and handle variable workloads.

---

### Question 5

What is Amazon Simple Notification Service (Amazon SNS)?

* [ ] A flexible and scalable outbound and inbound marketing communications service that uses email, short message service (SMS), push, or voice communication channels
* [ ] A serverless event bus that enables easy connection of applications by using data from your own applications, integrated software as a service (SaaS) applications, and AWS services
* [ ] A cost-effective, flexible, and scalable email service that enables developers to send email from within any application
* [x] **A fully managed messaging service for both system-to-system and application-to-person (A2P) communication, which uses publish/subscribe patterns**

**Rationale:**
**Amazon SNS** is a **fully managed, serverless messaging service** that supports the **publish/subscribe (pub/sub) pattern**. It allows publishers to send messages to multiple subscribers simultaneously through supported protocols such as **HTTP/S, email, SMS, Lambda, or SQS**. This makes it suitable for both **application-to-application (A2A)** and **application-to-person (A2P)** communication.

---

### Question 6

What are some use cases for Amazon Simple Notification Service (Amazon SNS)? (Select THREE.)

* [ ] Hold input until it can be processed in the order that it's received.
* [x] **Notify multiple systems that user input is ready for processing.**
* [ ] Trigger a single AWS Lambda function when an object is created in an Amazon S3 bucket.
* [ ] Gather streaming data from multiple systems.
* [x] **Send a text message to systems operators when unusual activity has been detected.**
* [x] **Send a push notification to mobile applications when a new software update is available.**

**Rationale:**
**Amazon SNS** supports **pub/sub messaging**, enabling a single message to fan out to multiple subscribers such as **SQS queues, Lambda functions, HTTP endpoints, SMS, and mobile push notifications**. Common use cases include **notifying multiple systems of an event**, **alerting operators**, or **pushing updates to mobile apps**. It is not used for ordered message processing (that’s SQS) or for streaming data ingestion (that’s Kinesis).

---

### Question 7

What are some features of Amazon Simple Notification Service (Amazon SNS)? (Select TWO.)

* [x] **Message delivery to a URL**
* [x] **Message delivery to an Amazon Simple Queue Service (Amazon SQS) queue**
* [ ] Recall of sent messages
* [ ] Providing strict message ordering with standard topics
* [ ] Guaranteed message delivery even when an endpoint isn't accessible

**Rationale:**
**Amazon SNS** supports **multiple delivery protocols**, including **HTTP/S endpoints (URLs)**, **Amazon SQS queues**, **email**, **SMS**, and **mobile push notifications**. It enables **publish/subscribe messaging**, where messages are delivered to all subscribed endpoints. SNS does not guarantee delivery if the endpoint is permanently inaccessible, and it does not recall sent messages or enforce strict ordering — that’s handled by **FIFO topics**, not standard ones.

---

### Question 8

Two different AWS Lambda functions must simultaneously process PDF files that are uploaded to an Amazon S3 bucket. The S3 event notification allows only one action when the PDF files are uploaded. Which solution provides the least complex way to process messages with both Lambda functions efficiently?

* [ ] Send the S3 event to Amazon MQ for distribution to both Lambda functions.
* [ ] Send the S3 event to an Amazon Simple Queue Service (Amazon SQS) queue that both Lambda functions poll.
* [ ] Upload two copies of each PDF file by using different object key prefixes.
* [x] **Send the S3 event to an Amazon Simple Notification Service (Amazon SNS) topic that both Lambda functions subscribe to.**

**Rationale:**
The simplest and most scalable solution is to **use Amazon SNS**. You can configure the **S3 event** to publish to an **SNS topic**, and both **Lambda functions** can **subscribe** to that topic. This setup ensures that both functions receive the same event simultaneously without complex message routing or duplicating files.


---

### Question 9

What is Amazon MQ?

* [ ] Identity broker service
* [ ] Data migration service
* [x] **Message broker service**
* [ ] Application monitoring service

**Rationale:**
**Amazon MQ** is a **managed message broker service** for **Apache ActiveMQ and RabbitMQ**. It enables applications to communicate with each other using industry-standard messaging protocols. It’s ideal for migrating existing message broker workloads to AWS without rewriting application code.


---

### Question 10

Which is a common use case for Amazon MQ?

* [ ] Connect a virtual private cloud (VPC) to an on-premises network.
* [ ] Decouple components in a new cloud-native application.
* [ ] Upload a standalone static website to AWS.
* [x] **Leverage an existing on-premises application that uses Apache ActiveMQ to communicate between microservices.**

**Rationale:**
**Amazon MQ** is best suited for **migrating or extending existing on-premises applications** that already use **message brokers like Apache ActiveMQ or RabbitMQ**. It allows seamless integration with AWS without requiring major architectural changes, making it ideal for hybrid or legacy system communication between microservices.
