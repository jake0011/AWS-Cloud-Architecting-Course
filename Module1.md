# Module 1: Welcome to AWS Academy Cloud Architecting

## Module Objectives
This module prepares you to:
- Understand the basic elements of the café business case.
- Learn the role and responsibilities of a cloud architect.

---

## Module Overview

### Sections Included:
- Course Overview
- Café Business Case Introduction
- Roles in Cloud Computing

These sections support the learning objectives of the module.

---

## Course Introduction

Welcome to the AWS Academy Cloud Architecting course. This course is designed to help you build the skills needed to design cloud architectures using AWS services.

---

## Course Objectives

By the end of this course, you will be able to:
- Apply AWS architectural principles and best practices to guide design decisions.
- Use AWS services to build scalable, reliable, and highly available infrastructure.
- Choose AWS managed services to improve flexibility and resilience.
- Optimize performance and reduce costs in AWS-based cloud environments.
- Secure access for users, applications, and data using AWS tools.
- Improve architectures using the AWS Well-Architected Framework.

### Architecture Patterns Covered:
- Three-tier architectures
- Serverless architectures
- Streaming architectures

These objectives align with the AWS Certified Solutions Architect – Associate (SAA-C03) certification.

---

## Certification Alignment

This course supports the knowledge areas outlined in the AWS Certified Solutions Architect – Associate exam guide (SAA-C03), including:
- Designing solutions using the AWS Well-Architected Framework.
- Building secure, resilient, high-performing, and cost-effective architectures.
- Evaluating and improving existing AWS solutions.

> Note: This course is not an exam prep course, but it aligns with the certification requirements. Additional resources are provided in the final module.

---

## Course Structure

The course is divided into 17 modules:

1. Welcome to AWS Academy Cloud Architecting  
2. Introducing Cloud Architecting  
3. Securing Access  
4. Adding a Storage Layer with Amazon S3  
5. Adding a Compute Layer Using Amazon EC2  
6. Adding a Database Layer  
7. Creating a Networking Environment  
8. Connecting Networks  
9. Securing User, Application, and Data Access  
10. Implementing Monitoring, Elasticity, and High Availability  
11. Automating Your Architecture  
12. Caching Content  
13. Building Decoupled Architectures  
14. Building Serverless Architectures and Microservices  
15. Data Engineering Patterns  
16. Planning for Disaster  
17. Bridging to Certification

Modules 2–3 review foundational cloud concepts. Modules 4–8 focus on architectural layers. Modules 9–16 emphasize design principles and best practices. Module 17 helps you prepare for certification.

---

## Recommended Starting Point

To succeed in this course, it’s recommended that you have:
- Completed AWS Academy Cloud Foundations or a similar course.
- A basic understanding of distributed systems and multi-tier architectures.
- Familiarity with networking concepts.

If you need a refresher, self-paced training options are available in the course resources. These include content from AWS Educate, which is free and accessible to learners aged 13 and above.

### Accessing AWS Educate:
- Register with an email address (no credit card required).
- Verification may take up to 24 hours.
- Follow the instructions in the "Accessing AWS Educate" PDF included in your course resources.

## Suggested Starting Point for the Course

To get the most out of this course, it's recommended that you have:

- Completed the AWS Academy Cloud Foundations course or an equivalent.
- A working understanding of distributed systems.
- Familiarity with multi-tier architectures.
- Basic knowledge of networking concepts.

Modules 2 and 3 are designed to review foundational cloud concepts. These are assumed to be already familiar to you, either through previous coursework or personal experience. Therefore, they do not go into deep technical detail.

This course builds on your existing knowledge of web-based architecture and focuses specifically on cloud compute architecture using AWS. It does not cover general web architecture topics like distributed systems or internet-based networks in depth.

---

## Additional Learning Resources

If you feel you need a refresher or want to explore foundational topics further, you can access self-paced learning options listed in the course resources. These include materials from **AWS Educate**, which is a free learning platform.

### About AWS Educate:
- Open to learners aged 13 and above.
- No credit card required—just an email address.
- Offers training content tailored to your goals, interests, and current knowledge level.
- Provides a wide range of foundational cloud computing topics.

### How to Access AWS Educate:
- Register for an AWS Educate account.
- Account verification may take up to 24 hours.
- Follow the instructions in the **"Accessing AWS Educate" PDF** included in your course resources.

---

## Café Business Case Introduction

Welcome to the AWS Academy Cloud Architecting course!

The hands-on challenge labs in this course are based on a fictional business scenario. This café business case helps you explore cloud computing concepts in a practical, relatable context. It demonstrates how technical solutions can address real-world business needs.

---

## Café Business Scenario

Frank and Martha, a retired couple, have opened a café and bakery to pursue their passion for baking and stay active in retirement. Their café is located at the base of their building and has become a hub for the local community. They enjoy serving neighbors and supporting local events with their baked goods and coffee.

As their business grows, they’ve started receiving interest from tourists and business travelers. With help from their staff and some AWS consultants who visit the café as customers, they begin exploring how cloud computing can support their expanding needs.

In each challenge lab, you’ll take on the role of a café staff member and work with AWS consultants to design cloud solutions that meet the café’s business goals.

---

## Café Team Members

### Frank
- Co-owner of the café
- Retired from the navy
- Passionate about baking
- Not technically inclined

### Martha
- Co-owner of the café
- Retired accountant
- Comfortable with spreadsheets
- Limited technical background

### Sofía
- Daughter of Frank and Martha
- Manages supply chain and daily operations
- Has programming experience
- Planning to study business administration

### Nikhil
- Part-time café employee
- Skilled in visual design
- Interested in cloud computing and web development
- May take on more responsibilities when Sofía starts university

Sofía recently learned about AWS and introduced the idea of using cloud services to automate parts of the business. She explained how AWS could help reduce manual tasks and improve customer experience.

---

## Café Visitors Who Are AWS Consultants

Three regular café visitors—friends of Sofía and Nikhil—are AWS consultants. They often discuss their interests in cloud computing and technology during their visits.

### Olivia
- AWS Solutions Architect
- Recently moved to the downtown area
- Expert in AWS and cloud technologies
- Former network engineer
- Strong background in database technologies

### Faythe
- AWS Developer
- Completed an AWS internship program
- Skilled in programming and applying technology to solve business problems
- Holds the AWS Certified Security – Specialty certification
- Interested in big data solutions
- Frequent visitor who enjoys Frank’s baked goods

### Mateo
- AWS SysOps Engineer
- Experienced in automation and fault-tolerant solution design
- Focuses on backup and disaster recovery
- Former developer
- Mentor to Faythe since her internship
- Enjoys helping others learn about cloud computing

---

## The Evolving Café Architecture

Throughout the challenge labs, you help Frank and Martha evolve their cloud architecture. Starting from a simple static website, the architecture grows into a secure, scalable, and resilient cloud-based application. Here's a summary of each version and its purpose:

|Version |Business Reason for Update| Technical Requirements and Architecture Update| 
| V1 | Launch a basic website |Host a static site on Amazon S3|
| V2 | Add dynamic content and online ordering | Deploy web app and database on Amazon EC2 |
| V3 | Reduce database maintenance and improve security | Separate web and database layers; migrate database to Amazon RDS in a private subnet |
| V4 | Strengthen web application security | Use Amazon VPC to configure public and private subnets |
| V5 | Handle increased traffic and improve availability | Add load balancer, enable auto scaling, distribute resources across two Availability Zones |
| V6 | Automate deployments across Regions | Use CloudFormation templates for version-controlled deployment of network and application layers |
| V7 | Add reporting and reduce operational overhead | Use AWS Lambda to generate scheduled reports from Amazon RDS |

---

## Roles in Cloud Computing

This section introduces common roles in cloud computing and the types of work they typically involve. Whether you're starting a career in cloud computing, transitioning into it, or working in a cloud-enabled organization, understanding these roles is essential.

### Common Cloud Roles:

- **Cloud Architect**: Designs cloud solutions and ensures they meet business and technical requirements.
- **Cloud Developer**: Builds applications using cloud services and APIs.
- **SysOps Administrator**: Manages and monitors cloud infrastructure, focusing on operations and reliability.
- **Security Specialist**: Ensures cloud environments are secure and compliant.
- **Data Engineer**: Designs and maintains data pipelines and analytics solutions in the cloud.
- **DevOps Engineer**: Automates deployments and integrates development with operations using cloud tools.

Understanding these roles helps you identify where your interests and skills might fit in the cloud computing landscape.

## Starting a Career in Cloud Computing

Whether you're beginning your career in cloud computing, transitioning from another IT role, or working in an organization that uses cloud technologies, it's important to understand the common roles and responsibilities in this field. These roles are performed by individuals, teams, or entire departments depending on the organization.

---

## IT Professional

### Common Skills and Responsibilities:
- Acts as a generalist and may manage entire applications.
- Oversees production environments.
- Highly technical, with varying levels of cloud experience.
- May specialize in areas like security or storage.

### Typical Job Titles:
- IT Administrator
- Systems Administrator
- Network Administrator

IT professionals usually have a broad skill set. They understand the infrastructure and components of applications but may not have deep expertise in specific cloud services. Their technical background is often strong, even if their cloud experience is limited.

---

## IT Leader

### Common Skills and Responsibilities:
- Leads teams of IT professionals.
- Manages daily operations and budgets.
- Stays informed about emerging technologies and selects tools for projects.
- Involved in early project stages, then delegates tasks to the team.

### Typical Job Titles:
- IT Manager
- IT Director
- IT Supervisor

IT leaders are decision-makers and managers. They guide technology choices and oversee implementation, often stepping back once the project is underway and letting their team handle the execution.

---

## Developer

### Common Skills and Responsibilities:
- Writes, tests, and debugs code.
- Focuses on application-level development.
- Works with APIs, SDKs, and sample code.
- May specialize in areas like security or storage.

### Typical Job Titles:
- Software Developer
- System Architect
- Software Development Manager

Developers are hands-on with code and technical documentation. They build applications and use cloud tools to accelerate development. Their work is essential to turning business requirements into functional software.

---

## DevOps Engineer

### Common Skills and Responsibilities:
- Builds and maintains cloud infrastructure.
- Follows cloud architect guidelines.
- Experiments to improve deployment processes.

### Typical Job Titles:
- DevOps Engineer
- Build Engineer
- Reliability Engineer

DevOps engineers bridge development and operations. They automate deployments, configure servers, and create repeatable infrastructure solutions. Their work ensures that applications run smoothly and reliably in the cloud.

---

## Cloud Architect

### Common Skills and Responsibilities:
- Stays current with cloud technologies and trends.
- Selects appropriate services to meet business goals.
- Provides documentation, tooling, and guidance to developers.
- Solves challenges related to cost, performance, reliability, and security.

### Typical Job Titles:
- Cloud Architect
- Systems Engineer
- Systems Analyst

Cloud architects design cloud-based solutions and choose technologies that align with business objectives. They create architectural diagrams and documentation to guide development teams, while allowing room for innovation. Their responsibilities align closely with the pillars of the **AWS Well-Architected Framework**, which is covered in detail throughout this course.

---
