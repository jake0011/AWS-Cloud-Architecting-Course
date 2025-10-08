

### Question 1

Which two statements about Amazon EC2 Auto Scaling are accurate?

* [x] It can launch Amazon EC2 instances in multiple Availability Zones.
* [x] It can launch new Amazon EC2 instances based on a schedule.
* [ ] It requires the customer to use Reserved Instances only.
* [ ] It can launch Amazon EC2 instances, but customers must terminate instances after they are no longer needed.

**Rationale:**
Amazon EC2 Auto Scaling manages instances across **Availability Zones** for high availability. It can scale dynamically or via **scheduled actions** for predictable workloads.

---

### Question 2

A DevOps engineer detected that the demand on a fleet of Amazon EC2 instances in an Auto Scaling group increases by a set amount on weekend days. Which type of scaling is the MOST appropriate in this case?

* [x] Scheduled scaling
* [ ] Manual scaling
* [ ] Dynamic scaling
* [ ] Predictive scaling

**Rationale:**
**Scheduled scaling** is designed for predictable workloads that follow a known pattern, such as weekly demand spikes. It automatically adjusts capacity based on defined schedules.

---

### Question 3

A DevOps engineer launches a fleet of Amazon EC2 instances in an Auto Scaling group behind an Application Load Balancer. The EC2 instances must maintain 50 percent average CPU utilization. Which type of scaling is appropriate to use based on CPU utilization usage?

* [x] Target tracking scaling
* [ ] Manual scaling
* [ ] Simple scaling
* [ ] Step scaling

**Rationale:**
**Target tracking scaling** maintains a target metric value (like 50% CPU utilization) automatically, increasing or decreasing capacity as workload changes.

---

### Question 4

How can a user vertically scale an Amazon RDS database?

* [x] By changing the instance class or size
* [ ] By sharding the database
* [ ] By adding read replicas
* [ ] By creating dedicated read and write nodes

**Rationale:**
**Vertical scaling** in Amazon RDS involves modifying the **DB instance class** to adjust compute and memory capacity without changing application logic.

---

### Question 5

How can an AWS customer horizontally scale an Amazon Aurora database?

* [x] By adding Aurora Replica instances by using Aurora Auto Scaling
* [ ] By changing the instance type
* [ ] By creating a scaling policy
* [ ] By creating Amazon CloudWatch alarms

**Rationale:**
**Horizontal scaling** adds or removes resources. Aurora supports this through **Aurora Auto Scaling**, which dynamically adjusts the number of replicas based on workload metrics.

---

### Question 6

How does Amazon DynamoDB perform automatic scaling?

* [x] It adjusts the provisioned throughput capacity in response to traffic patterns.
* [ ] It changes the instance type in response to changes in processing load.
* [ ] It adds and removes database instances in response to changes in traffic.
* [ ] It adds read replicas in response to increased read demand.

**Rationale:**
DynamoDB auto-scaling dynamically adjusts **provisioned throughput (RCUs/WCUs)** to handle workload spikes while minimizing cost.

---

### Question 7

A fleet of Amazon EC2 instances is launched in an Amazon EC2 Auto Scaling group. The instances run an application that uses a custom protocol on TCP port 42000. Connections from client systems on the internet must balance across the instances. Which load balancing solution is the best solution?

* [x] Network Load Balancer
* [ ] Classic Load Balancer
* [ ] Application Load Balancer
* [ ] Gateway Load Balancer

**Rationale:**
**Network Load Balancer (NLB)** operates at Layer 4 (TCP/UDP) and is ideal for high-performance load distribution of custom TCP traffic like port 42000.

---

### Question 8

A company must build a highly available website that uses server-side scripts to serve dynamic HTML. Which solution provides the HIGHEST availability for the LEAST cost and complexity?

* [x] An Auto Scaling group launches Amazon EC2 instances, which are served by an Application Load Balancer. DNS name resolution points to the load balancer.
* [ ] The customer deploys a second web server in another Region. Amazon Route 53 uses failover routing for disaster recovery (DR).
* [ ] An Auto Scaling group launches Amazon EC2 instances, which are served by a Network Load Balancer. Amazon Route 53 uses latency-based routing.
* [ ] Amazon S3 hosts the website. DNS name resolution points to the S3 bucket.

**Rationale:**
This architecture provides elasticity and fault tolerance with **EC2 Auto Scaling** across multiple Availability Zones and an **Application Load Balancer** for distributing traffic efficiently.

---

### Question 9

Users in location A connect to an application in Region A. Users in location B connect to the same application in Region B. If the application in Region A becomes unhealthy, traffic for location A must be redirected to the application in Region B. Which solution meets this requirement?

* [x] Use geolocation routing with failover records in Amazon Route 53.
* [ ] Use geoproximity routing and a Network Load Balancer that is attached to both Regions.
* [ ] Use latency-based routing in Amazon Route 53 with Amazon CloudWatch alarms.
* [ ] Use an Application Load Balancer with Amazon CloudWatch alarms.

**Rationale:**
**Geolocation routing** directs users based on their physical location, while **failover routing** ensures automatic redirection to healthy endpoints during outages.

---

### Question 10

A software engineer has created an AWS account for their own personal development and testing. They want the account to stay within the AWS Free Tier and to not generate unexpected costs. Which approach will work and will require the LEAST effort?

* [x] Create an Amazon CloudWatch alarm to send an email message when the account billing exceeds $0.
* [ ] Create an Amazon CloudWatch metric to monitor account billing and limit it to $0.
* [ ] Sign in to the AWS Management Console each month and check the billing dashboard.
* [ ] Create a service control policy (SCP) to restrict all services that are not included in the AWS Free Tier.

**Rationale:**
Setting up a **CloudWatch billing alarm** with **SNS email notifications** automates cost monitoring, ensuring users are alerted immediately when charges occur.
