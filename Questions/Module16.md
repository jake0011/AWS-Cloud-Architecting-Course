
### Question 1  
Which are definitions for recovery point objective (RPO) and recovery time objective (RTO)?  
- [x] RPO is the maximum acceptable data loss, measured in time. RTO is the maximum acceptable time until recovery.  
- [ ] RPO is the target time until recovery. RTO is the average amount of time to recover.  
- [ ] RPO is the maximum acceptable data loss, measured in bytes. RTO is the average amount of time required to recover.  
- [ ] RPO is the maximum acceptable data loss, measured in bytes. RTO is the maximum acceptable data loss, measured in time.  

**Rationale**: RPO defines how much data loss is acceptable in terms of time (e.g., last 15 minutes of data), while RTO defines how quickly systems must be restored after a failure.

---

### Question 2  
What can you do to quickly replicate or redeploy environments in a disaster?  
- [x] Use AWS CloudFormation templates to deploy duplicate environments in the same Region.  
- [ ] Use AWS CodeBuild to deploy application containers in a new virtual private cloud.  
- [ ] Use AWS Elastic Beanstalk to deploy a new virtual private cloud (VPC) and subnets in a different Region.  
- [ ] Use AWS OpsWorks to rebuild Amazon RDS instances.  

**Rationale**: AWS CloudFormation allows you to define infrastructure as code, making it fast and consistent to redeploy environments during disaster recovery.

---

### Question 3  
A company stores data in an Amazon S3 bucket. Which solution provides the most efficient way to ensure that all new and existing objects and metadata are copied to another Region for disaster recovery (DR)?  
- [x] Enable cross-Region replication on the bucket and copy existing objects onto themselves.  
- [ ] Create a workflow with AWS Step Functions and AWS Lambda to synchronize the buckets.  
- [ ] Use an AWS Lambda function to copy objects so that all object create events initiate the function.  
- [ ] Copy both bucket objects to the target bucket, and configure clients to write new files to both.  

**Rationale**: Cross-Region Replication (CRR) automatically replicates new objects. Existing objects must be manually re-uploaded to trigger replication.

---

### Question 4  
Which strategy is the most efficient for Amazon EC2 disaster recovery (DR)?  
- [x] Store essential data separately from the instance, and develop rapid rebuild processes for compute instances.  
- [ ] Rebuild instances by using Amazon Machine Images (AMIs) from AWS Marketplace.  
- [ ] Back up instances on a regular schedule.  
- [ ] Synchronize instances with standby instances on nearly a continuous basis.  

**Rationale**: Separating data from compute and using AMIs or templates allows fast recovery without the cost of running standby infrastructure.

---

### Question 5  
Which service provides automatic failover between multiple endpoints in support of a geographic disaster recovery (DR) strategy?  
- [x] Amazon Route 53  
- [ ] Elastic Load Balancing (ELB)  
- [ ] AWS Direct Connect  
- [ ] Amazon Virtual Private Cloud (Amazon VPC)  

**Rationale**: Route 53 supports health checks and DNS failover, making it ideal for geographic DR strategies.

---

### Question 6  
Which statement about the backup and restore disaster recovery (DR) pattern is true?  
- [x] Most cost-effective, but highest recovery time objective (RTO)  
- [ ] Most cost-effective, but highest recovery point objective (RPO)  
- [ ] Less cost effective, but lowest recovery time objective (RTO)  
- [ ] Less cost effective, but lowest recovery point objective (RPO)  

**Rationale**: Backup and restore is the cheapest DR strategy but takes the longest to recover systems and data.

---

### Question 7  
Which statements accurately describe the infrastructure characteristics of common disaster recovery (DR) patterns? (Select TWO.)  
- [x] Pilot light has minimal infrastructure that always runs. The rest of the infrastructure does not run until a disaster occurs.  
- [x] Warm standby has a scaled-down version of all infrastructure that scales as necessary and within pre-defined limits to meet the load when a disaster occurs.  
- [ ] Warm standby has a second fully functional set of infrastructure that runs all the time.  
- [ ] Pilot light runs with some infrastructure at full capacity. The rest scales up when a disaster occurs.  
- [ ] Pilot light has a scaled-down version of all infrastructure that runs until a disaster.  

**Rationale**: Pilot light keeps only essential services running. Warm standby maintains a partial environment that can scale up quickly during a disaster.

---

### Question 8  
What does the multi-site disaster recovery (DR) pattern involve?  
- [ ] The load is distributed across multiple geographically separated sites to reduce the impact of disasters.  
- [ ] Backups are stored in different sites so that they are protected if a disaster occurs.  
- [x] It involves automatic failover to a second fully functional, constantly operational system that is in another site.  
- [ ] It involves failover to another site that is not running until it is needed.  

**Rationale**: Multi-site (active-active) DR involves fully operational systems in multiple regions with automatic failover and load distribution.

---

### Question 9  
A company requires a disaster recovery (DR) solution for a business-critical application that provides a recovery time objective (RTO) and recovery point objective (RPO) in minutes. However, they do not want to pay for more than what they need. Which DR pattern would most likely meet these requirements?  
- [ ] Pilot light  
- [ ] Multi-site  
- [x] Warm standby  
- [ ] Backup and restore  

**Rationale**: Warm standby provides minute-level RTO/RPO with lower cost than multi-site by running a scaled-down environment that can scale up during a disaster.

---

### Question 10  
What does an AWS Storage Gateway enable you to do? (Select THREE.)  
- [x] Use Server Message Block (SMB) or Network File System (NFS) to connect to Amazon S3.  
- [x] Present cloud-based internet Small Computer Systems Interface (SCSI) block storage volumes to on-premises applications.  
- [x] Transfer backup jobs from tape or Virtual Tape Library (VTL) systems to the cloud.  
- [ ] Connect to Amazon S3 through an API.  
- [ ] Provide a fully managed elastic Network File System (NFS) endpoint to systems in a virtual private cloud (VPC) and on-premises.  
- [ ] Give applications in a virtual private cloud (VPC) access to on-premises block storage.  

**Rationale**: AWS Storage Gateway supports file, volume, and tape interfaces to connect on-premises environments to AWS cloud storage, enabling hybrid storage solutions.

