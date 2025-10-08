

### Question 1

Which are characteristics of an AWS Identity and Access Management (IAM) group? (Select TWO.)

*   [x] **New users added to a group inherit the group's permissions.**
*   [x] **A user can belong to more than one group.**
*   [ ] Permissions in a group policy always override permissions in a user policy.
*   [ ] A group can belong to another group.
*   [ ] A group can have security credentials.

**Rationale:**
IAM groups are designed to attach permissions efficiently to multiple users, meaning users added to a group inherit the group’s permissions. Furthermore, users are permitted to belong to multiple groups. Groups cannot be nested (a group cannot belong to another group), and they lack security credentials, meaning they cannot access services directly. When permissions conflict, an explicit deny in any IAM policy (user or group) overrides an explicit allow.

### Question 2

What is an advantage of using attribute-based access control (ABAC) over role-based access control (RBAC)?

*   ABAC requires less testing than RBAC.
*   ABAC permissions explicitly identify the resources that they protect.
*   ABAC permissions are more secure than RBAC permissions.
*   **ABAC will likely require fewer policies than RBAC.**

**Rationale:**
ABAC simplifies access control, often requiring only a single policy to define permissions. ABAC is highly scalable because it defines permissions based on attributes (tags) applied to resources and identities, eliminating the need to update multiple policies every time new resources are added, which is a major scaling issue with RBAC in large organizations.

### Question 3

A developer is a member of an AWS Identity and Access Management (IAM) group that has a group policy attached to it. The group policy allows access to Amazon S3 and Amazon EC2 and denies access to Amazon Elastic Container Service (Amazon ECS). The developer also has a user policy attached which allows access to Amazon ECS and Amazon CloudFront. Which option describes the user's access?

*   Access to Amazon S3 and Amazon EC2, but no access to Amazon ECS and Amazon CloudFront.
*   **Access to Amazon S3, Amazon EC2, and Amazon CloudFront, but no access to Amazon ECS.**
*   Access to Amazon ECS and Amazon CloudFront, but no access to Amazon S3 and Amazon EC2.
*   Access to Amazon S3, Amazon EC2, Amazon ECS, and Amazon CloudFront.

**Rationale:**
The key rule governing conflicting IAM policies is that **an explicit deny in an IAM policy overrides an explicit allow**. In this scenario:
1.  Access to S3 and EC2 is allowed by the group policy.
2.  Access to CloudFront is allowed by the user policy.
3.  Access to ECS is explicitly denied by the group policy while simultaneously allowed by the user policy. Since an explicit deny overrides an allow, the user is denied access to ECS.

### Question 4

What is a benefit of identity federation with the AWS Cloud?

*   It assigns roles to authenticated users to control their access to AWS resources.
*   **It enables the use of an external identity provider to authenticate workforce users and give them access to AWS resources.**
*   It eliminates the need for defining permissions in AWS Identity and Access Management (IAM) to secure the access to AWS resources.
*   It centralizes the storage and management of user identities inside of the AWS Cloud.

**Rationale:**
Identity federation is defined as a system of trust between an external Identity Provider (IdP) and AWS (the Service Provider). Workforce Identity Federation specifically allows human users authenticated via a corporate directory (the external IdP) to access AWS resources without requiring separate AWS credentials. The system relies on IAM roles and policies to define permissions, meaning it does not eliminate the need for IAM.

### Question 5

Which service enables identity federation for accessing a web application running in the AWS Cloud?

*   AWS CloudHSM
*   **Amazon Cognito**
*   AWS Key Management Service (AWS KMS)
*   AWS WAF

**Rationale:**
Amazon Cognito is a fully managed service for authentication, authorization, and user management in web/mobile applications. It supports application access identity federation for customer-facing web and mobile applications. Its components, User Pools and Identity Pools, handle sign-in via third-party IdPs (like Google or Facebook) or enterprise IdPs (SAML/OIDC) and assign temporary AWS credentials.

### Question 6

Which service helps centrally manage billing, control access, compliance and security, and share resources across multiple AWS accounts?

*   **AWS Organizations**
*   Amazon Cognito
*   AWS Identity and Access Management (IAM)
*   AWS Systems Manager

**Rationale:**
AWS Organizations is the account management service designed to consolidate and centrally manage multiple AWS accounts. Its features include consolidated billing for cost efficiency and the use of Service Control Policies (SCPs) to set centralized policy controls over AWS services across the entire organization, which simplifies security, compliance, and budgetary management.

### Question 7

A technology company has multiple production accounts grouped into a production organizational unit (OU) in AWS Organizations. The company wants to prevent all AWS Identity and Access Management (IAM) users in the production accounts from deleting AWS CloudTrail logs. How can a system administrator enforce this restriction?

*   Create a tag policy and attach it to the production accounts.
*   Create an Amazon S3 bucket policy and associate with all buckets containing AWS CloudTrail logs.
*   Create an IAM policy and attach it to each IAM user in the production accounts.
*   **Create a service control policy (SCP), and attach it to the production OU.**

**Rationale:**
Service Control Policies (SCPs) are used within AWS Organizations to define guardrails and set central policy control over maximum permissions across accounts. SCPs apply to entire accounts, cannot be overridden by local administrators, and an explicit deny in an SCP overrides any allow in an IAM policy. SCPs can be used specifically to block high-risk service access, such as denying the disabling of AWS CloudTrail, ensuring compliance organization-wide.

### Question 8

A developer is writing a client application that encrypts sensitive data using a data key before sending it to a server application. The client application sends the data key to the server application so that the server application can decrypt the sensitive information. The developer is concerned that the confidentiality of the sensitive data might be compromised if the data key is stolen. Which type of encryption should the developer use to fully protect the sensitive information?

*   Server-side encryption
*   Asymmetric encryption
*   **Envelope encryption**
*   Symmetric encryption

**Rationale:**
Envelope encryption addresses the need to securely transport a data key. This method involves encrypting the sensitive data with a fast symmetric data key, and then encrypting the data key itself using a key-encryption key (often asymmetric). The encrypted data key is stored alongside the encrypted data, meaning that if the entire package is compromised, the sensitive data remains protected because the data key required for decryption is also encrypted.

### Question 9

Which functions does the AWS Key Management Service (AWS KMS) provide? (Select TWO.)

*   [ ] Authenticate external users
*   [ ] Store encrypted data
*   [ ] Create AWS Identity and Access Management (IAM) access keys
*   [x] **Rotate keys**
*   [x] **Create symmetric and asymmetric keys**

**Rationale:**
AWS KMS is a managed service for creating and controlling cryptographic keys. Features include the ability to create and manage customer-managed keys with automatic rotation. Furthermore, KMS cryptographic operations support generating both symmetric data keys (`GenerateDataKey`) and asymmetric data key pairs (`GenerateDataKeyPair`). AWS KMS protects the keys using FIPS 140-2 validated Hardware Security Modules (HSMs).

### Question 10

Which AWS service discovers and protects sensitive information stored on Amazon S3 in an AWS account?

*   **Amazon Macie**
*   Amazon Detective
*   AWS Audit Manager
*   AWS Resource Access Manager (AWS RAM)

**Rationale:**
Amazon Macie is a data security service that uses machine learning and pattern matching to discover and protect sensitive data stored in Amazon S3. Macie specifically features automated sensitive data discovery for information such as PII, financial data, and credentials.
