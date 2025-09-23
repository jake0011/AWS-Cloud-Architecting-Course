# Module 9: Securing User, Application, and Data Access
This module focuses on securing user, application, and data access within AWS architectures, leveraging AWS Identity and Access Management (IAM), user federation, account management, encryption, and security services. Below, we outline the introduction, objectives, overview, labs, and key considerations for designing secure architectures.

---

## Introduction
This section describes the content of the module, emphasizing secure access management for users, applications, and data in AWS.

- **Module Objectives**:
  - Use AWS Identity and Access Management (IAM) users, groups, and roles to manage permissions.
  - Implement user federation within an architecture to increase security.
  - Describe how to manage multiple AWS accounts.
  - Recognize how AWS Organizations service control policies (SCPs) increase security within an architecture.
  - Encrypt data at rest by using AWS Key Management Service (AWS KMS).
  - Identify appropriate AWS security services based on a given use case.

- **Module Overview**:
  - **Presentation Sections**:
    - Managing permissions
    - Federating users
    - Managing access to multiple accounts
    - Encrypting data at rest
    - AWS security services for securing user, application, and data access
  - **Knowledge Checks**:
    - 10-question knowledge check delivered in the online course
    - Sample exam question for class discussion

- **Hands-on Labs in This Module**:
  - **Guided Labs**:
    - **Securing Applications by Using Amazon Cognito**: Configure Amazon Cognito for secure application access.
    - **Encrypting Data at Rest by Using AWS Encryption Options**: Implement encryption for data at rest.
    - Additional details are in the student guide, with instructions provided in the lab environment.

- **Cloud Architect Considerations**:
  - Design scalable permission schemes for users, applications, and data that align with security best practices.
  - Prevent unauthorized access to data and applications, ensuring protection of stored data.
  - Evaluate purpose-built AWS security services to optimize application security, reducing the burden on internal security teams.
  - Work backward from business needs (e.g., the fictional café scenario) to design secure architectures tailored to specific use cases.

---

## Section 2: Managing Permissions
This section explores how to manage permissions in AWS using AWS Identity and Access Management (IAM) groups, role-based access control (RBAC), and attribute-based access control (ABAC) to ensure scalable and secure access to resources. Below, we detail the challenges, solutions, and best practices for permissions management.

- **Challenge of Managing Permissions**:
  - Assigning permissions directly to individual IAM users is inefficient and difficult to scale.
  - **Example Scenario**:
    - Initial setup: Three developers (John, Mary, Pat) each have individual IAM policies granting full Amazon EC2 access.
    - Additional access: Granting Amazon S3 access requires modifying each user’s policy (three updates).
    - Scaling issue: With 100 developers, managing individual policies becomes impractical.

- **Use IAM Groups to Attach Permissions to Multiple Users**:
  - Create IAM groups based on job functions (e.g., developers group) and attach a single IAM policy to grant required permissions.
  - Users added to the group (e.g., John, Mary, Pat) inherit the group’s permissions.
  - Updates (e.g., adding Amazon S3 access) require modifying only the group’s policy, not individual user policies.
  - **IAM Group Characteristics**:
    - Add or remove users from groups.
    - Users can belong to multiple groups.
    - Groups cannot be nested.
    - Groups are granted permissions via access control policies but lack security credentials and cannot access services directly.
  - **Example**: Adding a new developer (Pat) to the developers group grants them the same EC2 and S3 access as others; moving Ana from the test group to the developers group updates her permissions efficiently.

- **Example: Using a User Policy and Group Policy Together**:
  - Scenario: Zhang has a user policy allowing Amazon S3 and DynamoDB access but denying Amazon Kinesis. Zhang is also in the Developer group, which allows S3, DynamoDB, Amazon Athena, and Kinesis.
  - **Resulting Permissions**:
    - Zhang can access Amazon Athena (allowed by group policy, not mentioned in user policy).
    - Zhang cannot access Amazon Kinesis (explicit deny in user policy overrides group policy allow).
  - Key Rule: An explicit deny in an IAM policy overrides an explicit allow.

- **Challenges of Scaling with Role-Based Access Control (RBAC)**:
  - RBAC defines permissions based on job functions, requiring:
    - Creating IAM policies listing specific resources for a role.
    - Attaching policies to IAM entities (users, groups, roles).
  - Scaling issue: Adding new resources requires updating multiple policies, which is time-consuming for large organizations.

- **Using Attribute-Based Access Control (ABAC)**:
  - ABAC defines permissions based on attributes (tags in AWS), which are key-value pairs applied to IAM resources (users/roles) and AWS resources.
  - **Benefits**:
    - More flexible than RBAC; no need to list individual resources in policies.
    - Granular permissions without updating policies for new users/resources.
    - Highly scalable and fully auditable.
  - **Example**: A development organization uses tags (e.g., `Env=dev`, `Project=maint`) for IAM roles and resources (EC2 instances, S3 buckets). A single policy grants access based on matching tags (e.g., developers with `Env=dev` and `Project=maint` access corresponding EC2 and S3 resources).
  - **Implementation Steps**:
    1. Apply tags to IAM identities (e.g., `Env=dev`, `Project=maint` for maintenance developers).
    2. Enforce tag requirements for new resources via policies.
    3. Configure permissions based on tag matches, denying access for non-matching tags.
    4. Test configurations to ensure resources cannot be created without required tags and that access is granted/denied correctly.

- **Tagging in AWS**:
  - Tags are metadata (key-value pairs) applied to AWS resources and IAM identities.
  - Uses: Billing, filtered views, access control (e.g., `Name=Web server`, `Project=Unicorn`, `Env=Dev` for an EC2 instance).
  - Limits: Up to 50 tags per resource; each tag key is unique and case-sensitive with one value.
  - Types: Technical (e.g., environment), business (e.g., department), security (e.g., data confidentiality level).

- **Key Takeaways: Managing Permissions**:
  - Use IAM groups to grant the same access rights to multiple users, creating groups that reflect job functions.
  - Use ABAC rather than RBAC to scale permissions management.
  - ABAC is an authorization strategy that defines permissions based on attributes, simplifying access control with a single policy.
  - Attributes are key-value pairs; AWS enables customers to assign attributes to resources and identities as tags.

---

## Section 3: Federating Users
This section explores identity federation in AWS, focusing on how to authenticate users and convey authorization information using AWS services like IAM, AWS IAM Identity Center, AWS Security Token Service (STS), and Amazon Cognito. Below, we detail the concepts, processes, and use cases for federating users to enhance security and access management.

- **Identity Federation**:
  - A system of trust between an **Identity Provider (IdP)** and a **Service Provider (SP)** to authenticate users and authorize access to resources.
  - **IdP**: Responsible for user authentication (e.g., OpenID Connect (OIDC) providers like Login with Amazon, Facebook, Google, or Security Assertion Markup Language (SAML) providers like Shibboleth, Active Directory Federation Services).
  - **SP**: Controls access to resources (e.g., AWS services, social media platforms, online banks).
  - Federation establishes trust through administrative agreements and configurations, allowing the SP to rely on the IdP for authentication.

- **AWS Services Supporting Identity Federation**:
  - **AWS Identity and Access Management (IAM)**: Enables federated access to AWS accounts using SAML 2.0 or OIDC IdPs, passing user attributes (e.g., cost center, title) for fine-grained access control. Reusable customer-managed IAM policies simplify access management across multiple accounts.
  - **AWS IAM Identity Center (Successor to AWS Single Sign-On)**: Centrally manages federated access to multiple AWS accounts and applications, providing a user portal for single sign-on (SSO). Supports IdPs like Okta or Azure AD via SAML 2.0, with permissions based on group membership.
  - **AWS Security Token Service (STS)**: Provides temporary, limited-privilege credentials for IAM users, federated users, or applications via the `AssumeRole` operation, enabling cross-account access or federation.
  - **Amazon Cognito**: A fully managed service for authentication, authorization, and user management in web/mobile apps, supporting sign-in via social IdPs (e.g., Amazon, Facebook, Google) or SAML, with user pools and identity pools for managing identities and permissions.

- **Workforce Identity Federation**:
  - Workforce identities are human users within an organization (e.g., employees using corporate credentials).
  - Federation allows users authenticated via a corporate directory (e.g., Microsoft Active Directory, LDAP) to access AWS resources without separate AWS credentials.
  - **Process**:
    1. User authenticates against a local directory with ID and password.
    2. An external system presents authentication information to IAM.
    3. IAM uses AWS STS to return temporary credentials.
    4. User accesses protected AWS resources with temporary credentials.

- **AWS IAM Identity Center**:
  - Centralizes identity management for AWS accounts and cloud applications, providing a unified administration experience.
  - Features:
    - Create or connect identities once and manage access across AWS accounts.
    - Assign fine-grained permissions based on job functions.
    - Offers a user portal for SSO access to assigned accounts and applications (e.g., Microsoft 365, Salesforce).
    - Can work alongside or replace IAM for account access management.
    - Integrates with external IdPs (e.g., Okta, Azure AD) via SAML 2.0.

- **AWS Security Token Service (STS)**:
  - Provides temporary credentials via API operations like `AssumeRole` for cross-account access or federation.
  - Credentials are time-limited (minutes to hours) and expire, enhancing security.
  - Used in federation to grant temporary access to AWS resources.

- **Identity Federation to AWS with an Identity Broker**:
  - An identity broker acts as an intermediary between an IdP and SP (e.g., AWS services).
  - **Process**:
    1. User signs in with existing IdP credentials (e.g., corporate login, Amazon.com ID).
    2. Identity broker requests temporary credentials from AWS STS.
    3. AWS STS generates time-limited credentials.
    4. Identity broker passes credentials to the application for user access.
  - Example (OIDC-based): User authenticates via corporate directory, identity broker requests STS credentials, and user is redirected to the AWS Management Console with temporary credentials for SSO.

- **Identity Federation for AWS Management Console Using SAML**:
  - For SAML 2.0-compatible corporate directories, configure SSO access to the AWS Management Console.
  - **Process**:
    1. User navigates to an internal portal (acting as the IdP).
    2. IdP authenticates user against a directory (e.g., LDAP, Active Directory).
    3. IdP returns a SAML assertion to the portal.
    4. Portal posts the SAML assertion to the AWS SAML sign-in endpoint, which uses AWS STS `AssumeRoleWithSAML` to generate temporary credentials and a sign-in URL.
    5. User is redirected to the AWS Management Console with temporary credentials.

- **Amazon Cognito**:
  - A fully managed service for authentication, authorization, and user management in web/mobile apps.
  - **Components**:
    - **User Pools**: User directories for sign-in via Amazon Cognito or third-party IdPs (e.g., Facebook, Google, SAML). Maintain user profiles accessible via SDKs.
    - **Identity Pools**: Generate unique identities and assign temporary AWS credentials via AWS STS for accessing AWS services.
  - Supports social sign-in (e.g., Amazon, Facebook, Google, Apple) and enterprise IdPs via SAML 2.0 or OIDC.
  - Scales to millions of users, providing secure access control.

- **Application Access Identity Federation with Amazon Cognito**:
  - Enables federation for customer-facing web/mobile apps.
  - **Process**:
    1. User signs in via Amazon Cognito user pool, receiving user pool tokens.
    2. Tokens control access to server-side resources or are exchanged via identity pools for AWS credentials to access other AWS services.
  - Features a hosted web UI for sign-up, sign-in, multi-factor authentication (MFA), and password reset, built on OAuth 2.0.
  - User pool groups manage permissions for different user types (e.g., readers, contributors).

- **Amazon Cognito User Pools**:
  - Acts as a user directory for local or federated users, issuing JSON Web Tokens (JWTs) based on OIDC standards.
  - **Features**:
    - **Sign-up**: Users create profiles in the user pool or via third-party IdPs; supports data imports.
    - **Sign-in**: Acts as a standalone IdP for local users or integrates with third-party IdPs.
    - **Federate Third-Party Identities**: Manages tokens from social (e.g., Facebook, Google) or SAML/OIDC IdPs, mapping claims to JWTs.
    - **Hosted UI**: Provides customizable web pages for sign-up, sign-in, MFA, and password reset.
    - **Support for JWTs**: Uses JWTs for server-side resource access or AWS credential exchange.
    - **User Pool Groups**: Organize users for permission management (e.g., readers, editors).
  - Manages user profiles via AWS Management Console, SDKs, or AWS CLI.

- **Key Takeaways: Federating Users**:
  - Identity federation is a system of trust between IdPs and SPs.
  - AWS IAM Identity Center provides a unified administration experience to define, customize, and assign fine-grained permissions based on common job functions.
  - AWS STS is a web service that provides temporary AWS credentials, allowing IAM users, federated users, or applications to assume IAM roles.
  - An identity broker facilitates federation for users with existing external identities (e.g., corporate directories).
  - Amazon Cognito is a fully managed service for authentication, authorization, and user management in web/mobile apps, supporting direct or third-party sign-in (e.g., Facebook, Amazon, Google).

---

## Section 4: Managing Access to Multiple Accounts
This section focuses on managing access across multiple AWS accounts using AWS Organizations, Service Control Policies (SCPs), permissions boundaries, and AWS Control Tower to ensure secure, scalable, and compliant architectures. Below, we detail the patterns, benefits, challenges, and tools for managing multiple accounts.

- **Two Common Patterns for Separating Resource Access**:
  - **Single AWS Account with Multiple VPCs**: Ideal for centralized security management with minimal overhead. Resources for different teams or departments are isolated within separate VPCs in one account.
  - **Multiple AWS Accounts with One VPC per Account**: Preferred by organizations for better isolation. Common use cases:
    - Separate accounts for business units/departments.
    - Separate accounts for development, test, and production environments.
    - Example: One account for common resources (e.g., DNS, Active Directory) and separate accounts for autonomous projects or departments.

- **Advantages and Challenges of Multiple Accounts**:
  - **Advantages**:
    - Isolation by business units, environments (e.g., dev, test, prod), or regulated workloads.
    - Isolation of auditing and recovery data.
    - Simplified cost tracking with alerts for each business unit.
    - Cost savings through consolidated billing and tiered pricing.
  - **Challenges**:
    - Security management across accounts requires consistent IAM policy replication, often needing custom automation or manual effort.
    - Manual account creation is time-consuming and hard to track.
    - Determining billing allocation for cost centers.
    - Need for centralized governance to ensure compliance and consistency.

- **AWS Organizations**:
  - An account management service to consolidate and centrally manage multiple AWS accounts.
  - **Features**:
    - Centralized account creation and management.
    - Consolidated billing for cost efficiency.
    - Hierarchical grouping of accounts into Organizational Units (OUs), nestable up to five levels.
    - Service Control Policies (SCPs) for centralized policy control over AWS services and API actions.
  - **Benefits**:
    - Simplifies security, compliance, and budgetary management.
    - Enables fine-grained policies for OUs or accounts, enhancing control over permissions.

- **AWS Organizations: Implementation Steps**:
  1. **Create a Hierarchy of OUs**: In the primary account, create OUs under the root container (automatically created by AWS Organizations).
  2. **Assign Accounts to OUs**: Assign member accounts to OUs (e.g., AWS Account #1 to Internal IT OU, #2 to Engineering OU, #3 to Development OU, #4 and #5 to Production OU).
  3. **Define SCPs**: Create SCPs to set permissions restrictions for specific accounts or OUs.
  4. **Attach SCPs**: Apply SCPs to the root, OUs, or individual accounts. SCPs applied to the root affect all OUs and accounts; SCPs applied to specific OUs or accounts are more targeted.
  - **Example SCP**: Prevent member accounts from leaving the organization:
    ```json
    {
      "Version": "2023-06-17",
      "Statement": [
        {
          "Effect": "Deny",
          "Action": ["organizations:LeaveOrganization"],
          "Resource": "*"
        }
      ]
    }
    ```

- **Organizations SCPs**:
  - Provide central control over maximum permissions for all accounts in an organization.
  - Define guardrails, limiting actions that account administrators can delegate to IAM users/roles.
  - **Characteristics**:
    - Apply to entire accounts, not individual resources.
    - Cannot be overridden by local administrators.
    - Work with IAM policies; permissions are granted only if both SCP and IAM policies allow the action.
    - An explicit deny in either SCP or IAM policy overrides an allow.
  - **Examples**:
    - Block service access (e.g., deny disabling AWS CloudTrail).
    - Enforce resource tagging (e.g., require specific tags for launching EC2 AMIs).
    - Prevent member accounts from leaving the organization.

- **Combining SCPs with IAM Identity-Based Policies**:
  - Effective permissions are the intersection of SCPs and IAM identity-based policies.
  - If either policy type explicitly denies an action, the request is denied.
  - Example: An SCP denies `ec2:*` for a test OU, and an IAM policy allows `ec2:DescribeInstances`. The user cannot perform EC2 actions due to the SCP’s explicit deny.

- **Permissions Boundaries**:
  - Set maximum permissions for IAM entities (users or roles) using a managed policy.
  - **Characteristics**:
    - Do not grant permissions; limit what identity-based policies can allow.
    - Example: A permissions boundary allows `s3:*`, `cloudwatch:*`, `ec2:*`. An IAM policy allowing `iam:CreateUser` fails because IAM is outside the boundary.
  - Effective permissions require both the permissions boundary and identity-based policy to allow the action; an explicit deny in either overrides an allow.

- **How Multiple Policy Types Impact Permissions**:
  - Example Scenario:
    - SCP (Test OU): Denies `ec2:*` and `sqs:*`.
    - Permissions Boundary: Allows `s3:*`, `sqs:*`.
    - Identity-Based IAM Policy: Allows `ec2:DescribeInstances`, `kms:*`, `s3:*`, `sqs:SendMessage`.
  - **Resulting Permissions**:
    - **EC2 Describe Instances**: Denied (SCP explicit deny overrides IAM policy allow).
    - **AWS KMS**: Denied (not in permissions boundary).
    - **Amazon S3**: Allowed (in permissions boundary, IAM policy, and no SCP deny).
    - **Amazon SQS Send Message**: Denied (SCP explicit deny overrides permissions boundary and IAM policy allow).
  - Effective permissions are the intersection of SCPs, permissions boundaries, and IAM policies.

- **Comparing Permissions Boundaries and SCPs**:
  - **Permissions Boundary**:
    - Applies to IAM entities (users/roles).
    - Defines maximum permissions for identity-based policies.
    - Example: Restrict a developer role to access only EC2, S3, and CloudWatch.
  - **Organizational SCP**:
    - Applies to all members of an organization or OU.
    - Defines maximum permissions for accounts.
    - Example: Deny Amazon RDS access to all members of an Internal IT OU.
  - Neither grants permissions; both set limits. Permissions boundaries are user/role-specific, while SCPs apply broadly to accounts.

- **AWS Control Tower**:
  - Facilitates setup and governance of a secure, multi-account AWS environment.
  - **Features**:
    - Automated setup of a well-architected multi-account landing zone based on best practices blueprints.
    - Governance via guardrails (high-level rules for security, operations, and compliance).
    - Prescriptive guidance for managing AWS environments at scale.
  - **Benefits**:
    - Simplifies multi-account setup and management.
    - Ensures compliance with built-in guardrails.
    - Ideal for new AWS environments, cloud initiatives, or existing multi-account setups needing standardized governance.

- **Key Takeaways: Managing Access to Multiple Accounts**:
  - Most organizations use multiple AWS accounts with one VPC per account for isolation and security.
  - Multiple accounts enable billing consolidation for cost savings and resource separation.
  - AWS Organizations consolidates multiple accounts into a centrally managed organization.
  - SCPs set permission limits across organizations, while permissions boundaries limit IAM entities.
  - Users or groups require explicit permissions from both SCPs and IAM policies; an explicit deny in any policy overrides an allow.

---

## Section 5: Encrypting Data at Rest.
This section explores encrypting data at rest using AWS Key Management Service (AWS KMS), covering the importance of encryption, types of encryption, and integration with AWS services like Amazon S3 and Amazon EBS. Below, we detail the concepts, processes, and best practices for securing data at rest.

- **Why Protect Data at Rest?**:
  - Ensures **confidentiality** (keeping data hidden from unauthorized parties) and **integrity** (preventing unauthorized modification) of information.
  - Provides an extra layer of protection if a system is compromised.
  - Addresses business or compliance requirements for data security.
  - Aligns with the **CIA triad** (Confidentiality, Integrity, Availability) for enterprise data security:
    - **Confidentiality**: Protects sensitive data from unauthorized access.
    - **Integrity**: Ensures data remains unaltered during storage or use.
    - **Availability**: Ensures data is accessible to authorized users when needed.

- **What is Data Encryption?**:
  - Encryption uses a cipher (algorithm) and a key to convert readable data (plaintext) into unreadable data (ciphertext), reversible only with the correct key.
  - Example: "Hello World!" may become "1c28df2b595b4e30b7b07500963dc7c" when encrypted.
  - Strong encryption algorithms rely on mathematical properties to resist decryption without the key, making key management critical.

- **Types of Encryption**:
  - **Symmetric Encryption**:
    - Uses the same key for encryption and decryption.
    - Faster and efficient for large datasets.
    - Widely used and considered secure with frequent key rotation.
    - Example: TLS protocol uses symmetric encryption for data exchange.
    - **Use Cases**:
      - Prioritizing speed, cost, and low computational overhead.
      - Encrypting large amounts of data.
      - Data staying within the organization’s network.
  - **Asymmetric Encryption**:
    - Uses a key pair: a public key for encryption and a private key for decryption.
    - Slower due to longer key lengths and complex calculations but considered more secure.
    - Supports **non-repudiation** (prevents denial of actions, as only the private key holder can decrypt data encrypted with their public key).
    - **Use Cases**:
      - Sharing data outside the organization.
      - Regulations prohibiting key sharing.
      - Non-repudiation requirements.
      - Segregating key access by organizational roles.
  - **Envelope Encryption**:
    - Combines symmetric and asymmetric encryption:
      1. Encrypt data with a symmetric data key.
      2. Encrypt the data key with another key (key-encryption key), often using asymmetric encryption.
      3. Add additional layers of encryption keys as needed.
      4. Store the encrypted data key with the encrypted data.
    - Analogy: Locking valuables in a safe, then locking the safe key in a safety deposit box.
    - Example: TLS (SSL) uses envelope encryption for secure data exchange.

- **Server-Side Encryption (SSE)**:
  - Encryption performed at the destination by the receiving AWS service.
  - Example: Uploading unencrypted data over HTTPS to Amazon S3, where S3 encrypts the data before storage, handling encryption and key management transparently.

- **AWS Key Management Service (AWS KMS)**:
  - A managed service for creating and controlling cryptographic keys.
  - **Features**:
    - Create and manage customer-managed keys with aliases, descriptions, and automatic rotation.
    - Import custom keys or use AWS-generated keys.
    - Uses **FIPS 140-2 validated hardware security modules (HSMs)** to protect keys.
    - Integrates with AWS services for seamless encryption.
    - Sets usage policies to control which users can access keys.
    - Logs all key operations in AWS CloudTrail for auditing.
  - Keys never leave AWS KMS unencrypted, reducing compromise risks.

- **AWS KMS Key Types**:
  - **Customer Managed Keys**: Created, owned, and managed by you in your AWS account.
  - **AWS Managed Keys**: Created and managed by AWS services for your account.
  - **AWS Owned Keys**: Managed by AWS services across multiple accounts, not visible in your account.
  - **Data Key (Symmetric)**: Used for encrypting large datasets outside AWS KMS; returned in plaintext and encrypted forms.
  - **Data Key Pair (Asymmetric)**: Includes a public/private key pair; private key remains in AWS KMS unencrypted.

- **AWS KMS Cryptographic Operations**:
  - **Encrypt**: Encrypts up to 4,096 bytes of data using a symmetric or asymmetric KMS key.
  - **Decrypt**: Decrypts ciphertext encrypted by a KMS key.
  - **GenerateDataKey**: Generates a symmetric data key for use outside AWS KMS, returning plaintext and encrypted copies.
  - **GenerateDataKeyPair**: Generates an asymmetric data key pair, returning plaintext public/private keys and an encrypted private key.

- **AWS KMS Integration with AWS Services**:
  - Supports only symmetric encryption for integrated services.
  - **Amazon S3**:
    - Uses server-side encryption (SSE-KMS) to encrypt objects.
    - Process:
      1. User uploads a file to an S3 bucket.
      2. S3 requests a data key from AWS KMS.
      3. KMS generates a plaintext data key and encrypts it with a customer-managed key.
      4. S3 receives both keys, encrypts the object with the plaintext data key, stores the encrypted key in object metadata, and deletes the plaintext key.
      5. For decryption, S3 sends the encrypted data key to KMS, which decrypts it and returns the plaintext key to S3 for object decryption.
  - **Amazon EBS**:
    - Encrypts volumes, disk I/O, and snapshots using AES-256-XTS (512-bit key equivalent).
    - Process:
      1. EBS obtains an encrypted data key from KMS, stored with the encrypted data.
      2. EC2 host servers retrieve the encrypted data key.
      3. KMS decrypts the data key over TLS using an HSM.
      4. The decrypted data key is used in memory to encrypt/decrypt EBS volume data; the encrypted key is retained for future use.
    - Transparent encryption/decryption with no additional user action required.

- **Key Takeaways: Encrypting Data at Rest**:
  - Encrypting data at rest enhances security by making it harder for attackers to compromise data, even if an endpoint is breached.
  - **Symmetric encryption** uses the same key for encryption and decryption, ideal for speed and large datasets.
  - **Asymmetric encryption** uses a public/private key pair, offering enhanced security and non-repudiation but slower performance.
  - **Envelope encryption** encrypts data with a data key and encrypts the data key with another key for added security.
  - **Client-Side Encryption (CSE)**: Applications encrypt data locally before sending to AWS and decrypt after retrieval.
  - **Server-Side Encryption (SSE)**: AWS services encrypt data at the destination.
  - **AWS KMS keys** are the primary resource for encryption, decryption, and re-encryption, integrated with services like S3 and EBS for secure data protection.

---

## Section 6: AWS Security Services for Securing User, Application, and Data Access
This section highlights AWS security services that enhance security best practices and protect all layers of your applications, including identity and access management, data protection, network security, and threat detection/response. Below, we detail key services, their features, use cases, and how they contribute to a defense-in-depth strategy.

- **AWS Services for Security, Identity, and Compliance**:
  - AWS offers services categorized to address various security needs:
    - **Identity and Access Management**: Securely manage identities, resources, and permissions at scale.
      - Examples: AWS IAM, AWS IAM Identity Center, Amazon Cognito, AWS Organizations.
    - **Detection and Response**: Enhance security posture and streamline operations.
      - Examples: AWS CloudTrail, Amazon Detective, Amazon Inspector, AWS Security Hub.
    - **Network and Application Protection**: Enforce fine-grained security policies at network control points.
      - Examples: AWS Network Firewall, AWS Shield, AWS WAF.
    - **Data Protection**: Protect data, accounts, and workloads from unauthorized access.
      - Examples: AWS KMS, AWS Secrets Manager, Amazon Macie.
    - **Compliance**: Monitor compliance status with automated checks based on AWS best practices and industry standards.
      - Examples: AWS Artifact, AWS Audit Manager.
  - For a complete list, see: [AWS Security Services](https://aws.amazon.com/products/security/).

- **Defense-in-Depth Approach**:
  - Aligns with the AWS Well-Architected Security Pillar, applying security at all layers.
  - Key layers include limiting access (covered in prior sections), blocking unwanted traffic, protecting data, and automating threat detection/response.
  - AWS security services reduce the need for custom code to implement these protections.

- **AWS WAF (Web Application Firewall)**:
  - **Description**: Monitors HTTP/HTTPS requests to protected web application resources, filtering malicious traffic.
  - **Features**:
    - Use managed or custom rules to allow/block requests based on IP address, country, header values, etc.
    - Integrates with AWS Shield (included at no cost) for protection against Distributed Denial of Service (DDoS) attacks at network, transport (Layer 3/4), and application (Layer 7) layers.
  - **Example Use Cases**:
    - Block requests missing the HTTP User-Agent header.
    - Detect and manage malicious account creation attempts on application sign-up pages.

- **Amazon Macie**:
  - **Description**: A data security service using machine learning and pattern matching to discover and protect sensitive data in Amazon S3.
  - **Features**:
    - Automated sensitive data discovery (e.g., PII like passport numbers, financial data, credentials).
    - Create/run sensitive data discovery jobs with built-in or custom data identifiers.
    - Review, analyze, and manage findings via dashboards and alerts.
    - Integrates with AWS Organizations to manage up to 5,000 accounts or 1,000 member accounts with a single Macie administrator account.
  - **Example Use Case**:
    - Identify sensitive data migrated to S3, notifying administrators to review and decide whether to allow continued storage.

- **Amazon Inspector**:
  - **Description**: A vulnerability management service that continuously scans AWS workloads (EC2 instances, Amazon ECR container images, AWS Lambda functions) for software vulnerabilities and unintended network exposure.
  - **Features**:
    - Centralized management via AWS Organizations.
    - Accurate vulnerability assessment with Amazon Inspector Risk score.
    - Dashboard for high-impact findings.
    - Integration with Amazon EventBridge for automated workflows.
  - **Example Use Case**:
    - Scan EC2 AMIs for vulnerabilities and generate reports to ensure AMIs are updated before deployment.

- **Amazon Detective**:
  - **Description**: Analyzes and investigates security findings or suspicious activities, identifying root causes using machine learning, statistical analysis, and graph theory.
  - **Features**:
    - Pre-built graph model summarizing security-related relationships and behavioral insights.
    - Validates, compares, and correlates data for faster investigations.
    - Automatically ingests data from enabled accounts, with up to one year of historical event data.
  - **Example Use Case**:
    - Triage issues by analyzing all activity related to a specific IAM entity, linked to AWS GuardDuty findings.

- **AWS Security Hub**:
  - **Description**: Collects and aggregates security data from AWS accounts, services, and third-party products to monitor security trends and prioritize issues.
  - **Features**:
    - Supports AWS Foundational Security Best Practices (FSBP) and external compliance frameworks.
    - Receives findings from services like Amazon Macie and Amazon Inspector.
    - Uses automation rules to update critical findings when security checks fail.
    - Integrates with Amazon EventBridge for automated response and remediation workflows.
    - Provides a security score for enabled standards and accounts.
  - **Example Use Case**:
    - Prioritize response and remediation by correlating and aggregating security findings across accounts and resources.

- **AWS Trusted Advisor**:
  - **Description**: Provides recommendations based on AWS best practices in five categories: cost optimization, security, fault tolerance, service limits, and performance improvement.
  - **Features**:
    - Evaluates accounts for optimization and security improvements.
    - Accessible via AWS Management Console for all support tiers.
    - Integrates with AWS Security Hub to display security controls and findings.
    - **Support Tiers**:
      - Basic/Developer Support: Access to core security checks and service quota checks.
      - Business/Enterprise Support: Access to all checks (cost, security, fault tolerance, performance, quotas).
  - **Example Use Case**:
    - Automate identification of security gaps and resource optimization opportunities for administrators, saving time compared to manual reviews.

- **Key Takeaways: AWS Security Services**:
  - AWS security services enable a defense-in-depth strategy for AWS workloads.
  - Key services include:
    - **AWS WAF**: Monitors and filters web requests to protect applications.
    - **Amazon Macie**: Identifies and protects sensitive data in Amazon S3.
    - **Amazon Inspector**: Scans for vulnerabilities in EC2, containers, and Lambda functions.
    - **Amazon Detective**: Investigates security findings using data visualizations.
    - **AWS Security Hub**: Consolidates findings to monitor security posture.
    - **AWS Trusted Advisor**: Provides recommendations to close security gaps and optimize resources.

---

### Module summary
This module prepared you to do the following: 
- Use AWS Identity and Access Management (IAM) users, groups, and roles to manage permissions.
- Implement user federation within an architecture to increase security.
- Describe how to manage multiple AWS accounts.
- Recognize how AWS Organizations service control policies (SCPs) increase security within an architecture.
- Encrypt data at rest by using AWS Key Management Service (AWS KMS).
- Identify appropriate AWS security services based on a given use case.
