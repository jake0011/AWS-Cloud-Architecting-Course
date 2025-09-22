# Module 3: Securing Access

## Security in the AWS Well-Architected Framework

Security is one of the six pillars of the AWS Well-Architected Framework. The other pillars include:

- Operational Excellence
- Reliability
- Performance Efficiency
- Cost Optimization
- Sustainability

The framework provides best practices and design guidance to help you evaluate and improve cloud architectures. You can use the **AWS Well-Architected Tool (WA Tool)** to implement these best practices across all pillars, including security.

For more details, refer to the AWS Well-Architected Framework site and the appendix of questions and best practices available in your course resources.

---

## Security Pillar: Design Principles

To strengthen your architecture’s security posture, follow these key design principles:

1. **Implement a Strong Identity Foundation**  
   - Use least privilege access.
   - Centralize identity management.
   - Avoid long-term static credentials.

2. **Protect Data in Transit and at Rest**  
   - Classify data by sensitivity.
   - Use encryption, tokenization, and access controls.

3. **Apply Security at All Layers**  
   - Use a defense-in-depth approach.
   - Secure every layer: network edge, VPC, load balancer, compute, OS, application, and code.

4. **Keep People Away from Data**  
   - Use automation and tools to reduce manual data access.
   - Minimize human error and risk.

5. **Maintain Traceability**  
   - Monitor, alert, and audit changes in real time.
   - Integrate logging and metrics with automated investigation systems.

6. **Prepare for Security Events**  
   - Establish incident response policies.
   - Run simulations and automate detection and recovery.

7. **Automate Security Best Practices**  
   - Use software-based controls defined as code.
   - Implement version-controlled templates for secure architecture deployment.

---

## User Permissions and Identity Management

### Example:
- **John**: Can read, write, and delete objects in **S3 Bucket 1**; read-only access to **S3 Bucket 2**; denied access to a specific **DynamoDB table**.

Use **policies** to grant or restrict access to AWS resources. This is part of building a strong identity foundation.

---

## Principle of Least Privilege

### Best Practices:
- Grant only the permissions needed for a task.
- Start with minimal permissions and expand as necessary.
- Revoke permissions that are no longer needed.

### Example:
- **John (Admin)**: Full access to S3 Buckets 1 and 2.
- **Mary (Marketing)**: Read-only access to S3 Bucket 1; explicitly denied access to S3 Bucket 2.

Design policies based on specific user roles and tasks. Avoid overly permissive access.

---

## Use Encryption

Protect data in transit using cryptographic protocols like **TLS**.

### Key Concepts:
- **Data in transit**: Actively moving across networks or between devices.
- Use encryption to ensure privacy and security during transfer.
- Example: Encrypting images during upload to cloud storage using TLS.

---

## Protecting Data at Rest with Client-Side Encryption

**Client-side encryption** means the client encrypts data before sending it to the cloud and decrypts it after retrieving it.

### Key Concepts:
- Provides **end-to-end protection** for data in transit and at rest.
- Encryption occurs **before** data leaves the client device.
- Decryption occurs **after** data is retrieved from cloud storage.

### Example:
Encrypting sensitive data on a mobile device ensures that even if the device is lost or stolen, the data remains inaccessible to unauthorized users.

---

## Protecting Data at Rest with Server-Side Encryption

**Server-side encryption** means AWS encrypts data as it is stored and decrypts it when accessed.

### Key Concepts:
- Data is sent unencrypted through a secure channel.
- AWS encrypts the data **at rest** in the cloud.
- AWS decrypts the data **on request** and returns it securely.

### Example:
Amazon S3 encrypts personal data (e.g., Social Security Numbers) at the object level when storing it in AWS data centers.

---

## Key Takeaways: Security Principles

- **Security and compliance** are shared responsibilities between AWS and the customer.
- The **Security Pillar** of the AWS Well-Architected Framework provides design principles for secure cloud architecture.
- The **Principle of Least Privilege** is essential for strong identity management.
- **Encryption** is a critical mechanism for protecting data both **in transit** and **at rest**.

---

## Using IAM to Control Access to AWS Resources

**AWS Identity and Access Management (IAM)** allows you to manage access to AWS resources by creating and assigning permissions to users, groups, and roles.

### Key IAM Components:
- **IAM User**: Represents a person or application with permanent credentials.
- **IAM Group**: A collection of users with shared permissions.
- **IAM Role**: Grants temporary permissions to users or applications.
- **IAM Policy**: A document that defines access permissions for resources.

Policies can be attached to users, groups, or roles to enforce access control.

---

## IAM Credentials for Authentication

Different credentials are used depending on how you access AWS:

ActionCredentials NeededSign in to AWS ConsoleUsername and password| Use AWS CLI or SDKs | AWS access key (ID + secret key) |

> Note: AWS access keys are used for programmatic access and must be kept secure.

---

## Best Practices to Secure Access

Follow these best practices to enhance security:

- **Principle of Least Privilege**: Grant only the permissions needed for tasks.
- **Enable MFA**: Adds an extra layer of protection.
- **Use Temporary Credentials**: Prefer IAM roles over long-term credentials.
- **Rotate Access Keys**: Regularly update keys for long-term use cases.
- **Use Strong Passwords**: Follow AWS password policy guidelines.
- **Secure Local Credentials**: Store credentials securely (e.g., password manager).
- **Use AWS Organizations**: Centralize account and resource management.
- **Enable AWS CloudTrail**: Monitor and audit account activity.
- **Protect the Root User**: Limit usage and monitor activity closely.

---

## Protecting the Root User

The **root user** has full access to all AWS services and resources.

### Best Practices:
- Treat root credentials like sensitive personal data.
- Use **AWS IAM Identity Center** to create an admin user for daily tasks.
- Only use the root user for tasks that cannot be performed by other users.

Refer to the **Tasks That Require Root User Credentials** guide for more details.

---

## Steps to Set Up an Admin User

1. Log in as the root user and enable MFA.
2. Create a new admin user (e.g., John), enable MFA, and download programmatic keys.
3. Log out as the root user.
4. Log in as the admin user.
5. Create individual user accounts (e.g., John, Ji, Nikki) with specific policies.

> Use IAM Identity Center for centralized access management and temporary credentials.

---
## Best Practices: IAM Users and Groups

To manage long-term access securely:

- **Attach IAM policies to IAM groups**.
- **Assign IAM users to IAM groups**.

### Example:
- **Sales Group**: IAM Policy 1 attached; Users 1 and 2 inherit permissions.
- **IT Group**: IAM Policy 2 attached; Users 3, 4, and 5 inherit different permissions.

You can also attach policies directly to individual users for custom access control.

---

## IAM Roles

IAM roles provide **temporary security credentials** and are not tied to a specific user.

### Characteristics:
- Temporary credentials
- Can be assumed by users, applications, or services
- Ideal for delegation and cross-account access

### Common Use Cases:
- Applications running on **Amazon EC2**
- **Cross-account access** for IAM users
- **Mobile apps** needing secure access to AWS resources

Roles allow secure, temporary access without storing long-term credentials.

---

## Examples of Using IAM Roles

### 1. IAM User Accessing EC2:
- Create IAM policy with required permissions.
- Attach policy to IAM role.
- IAM user assumes the role to access EC2.

### 2. EC2 Instance Accessing S3:
- Create IAM role with S3 access permissions.
- Add role to an instance profile.
- Attach profile to EC2 instance.
- Application on EC2 assumes the role to access S3.

### 3. Cross-Account Access:
- IAM user in Account 2 needs access to S3 in Account 1.
- Create cross-account IAM role in Account 1.
- Define Account 2 as a trusted entity.
- IAM user in Account 2 switches roles to access S3 in Account 1.

---

## Key Takeaways: Authenticating and Securing Access

- Use **IAM** for fine-grained access control.
- Avoid using the **root user** for daily tasks.
- Attach IAM policies to **groups**, then assign users to those groups.
- Use **IAM roles** to grant temporary access to users, applications, or services.

---
## Authorizing Users

This section focuses on how AWS Identity and Access Management (IAM) policies are used to authorize users and control access to AWS resources.

---

## IAM Policies and Permissions

Policies define what actions a principal (user, role, or service) can perform on AWS resources.

### Two Types of Policies:
- **Identity-based policies**: Attached to IAM users, groups, or roles.
- **Resource-based policies**: Attached directly to AWS resources.

### Key Features:
- Policies are written in **JSON** format.
- They specify **allowed or denied actions** on resources.
- Always follow the **principle of least privilege** to minimize risk.

---

## Determining Permissions at Request Time

When a principal makes a request, AWS evaluates all applicable policies using the following logic:

1. **Default Deny**: All requests are denied unless explicitly allowed.
2. **Explicit Allow**: Overrides the default deny.
3. **Explicit Deny**: Overrides any explicit allow.

> If a conflict exists between allow and deny, **deny takes precedence**.

Use the **IAM Policy Simulator** to test and troubleshoot policies before applying them.

---

## Identity-Based vs. Resource-Based Policies

### Identity-Based Policies
Attached to IAM users, groups, or roles. Define **what an identity can do**.

#### Example:
IdentityResourceReadWriteListCarlosResource X✅✅✅| Richard  | Resource Y | ✅ | ❌ | ❌ |
| Managers | Resource Z | ❌ | ❌ | ✅ |

### Resource-Based Policies
Attached to AWS resources. Define **who can access the resource** and what they can do.

#### Example:
ResourceUserReadWriteListResource XAna✅✅✅| Resource Y | Paulo | ✅ | ✅ | ✅ |
| Resource Y | Nikki | ✅ | ❌ | ❌ |

> “N/A” means the policy doesn’t specify permissions for that action.

Use identity-based policies for internal access control and resource-based policies for cross-account or external access.

## Policies: Example 1 – Conflicting Permissions

### Identity-Based Policy (Attached to Bob)
| Resource | Action | Permission |
|----------|--------|------------|
| Bucket X | GET    | ✅ Allow   |
| Bucket X | PUT    | ✅ Allow   |
| Bucket X | LIST   | ✅ Allow   |
| Bucket Y | LIST   | ✅ Allow   |

### Resource-Based Policy (Attached to Bucket X)
UserActionPermissionBobGET✅ AllowBobPUT❌ Deny| Bob  | LIST   | ✅ Allow   |

### Result:
Even though Bob’s identity-based policy allows **PUT** on Bucket X, the resource-based policy **explicitly denies** it. Therefore, **Bob cannot PUT objects** into Bucket X.

---

## Policies: Example 2 – Permissions from Resource-Based Policy

### Identity-Based Policy (Attached to Bob)
ResourceActionPermissionBucket XGET✅ AllowBucket XPUT✅ AllowBucket XLIST✅ Allow| Bucket Y | LIST   | ✅ Allow   |

### Resource-Based Policy (Attached to Bucket Y)
| User | Action | Permission |
|------|--------|------------|
| Bob  | GET    | ✅ Allow   |
| Bob  | LIST   | ✅ Allow   |
| Bob  | PUT    | N/A        |

### Result:
Bob’s identity-based policy does **not explicitly allow** GET on Bucket Y, but the resource-based policy **does allow** it. Therefore, **Bob can GET and LIST objects** from Bucket Y.

---

## Key Takeaways: Authorizing Users

- A **policy** defines permissions for the identity or resource it’s attached to.
- IAM supports two types of policies:
  - **Identity-based**: Attached to IAM users, groups, or roles.
  - **Resource-based**: Attached directly to AWS resources.
- **Permissions** in policies determine whether a request is allowed or denied.
- **Evaluation Logic**:
  - All requests are **denied by default**.
  - An **explicit allow** overrides the default deny.
  - An **explicit deny** overrides any explicit allow.

Use the **IAM Policy Simulator** to test and validate permissions before deployment.

---

## Part of the AWS IAM Policy

### 1. IAM Policy Document Structure

- **Version**:  
  Specifies the version of the policy language (e.g., `"2012-10-17"`).

- **Statement**:  
  The main container for policy rules. A policy can have one or more statements.

  - **Effect**:  
    Determines if the statement allows (`"Allow"`) or denies (`"Deny"`) access.

  - **Principal**:  
    - Used in **resource-based policies** to specify the account, user, role, or federated user that the policy applies to.
    - Not used in **identity-based policies** (the principal is implied as the user/role the policy is attached to).

  - **Action**:  
    Lists the actions that are allowed or denied (e.g., `"s3:GetObject"`).

  - **Resource**:  
    Specifies the AWS resources the actions apply to (using ARNs).

  - **Condition** (optional):  
    Specifies conditions that must be met for the statement to apply.

- **Evaluation Logic**:  
  If a policy has multiple statements, AWS evaluates them using a logical **OR**.  
  An explicit **Deny** always overrides an Allow.

---

### 2. Example: Resource-Based Policy

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["dynamodb:*", "s3:*"],
      "Resource": [
        "arn:aws:dynamodb:region:account-number:table/course-notes",
        "arn:aws:s3:::course-notes-web",
        "arn:aws:s3:::course-notes-mp3/*"
      ]
    },
    {
      "Effect": "Deny",
      "Action": ["dynamodb:*", "s3:*"],
      "NotResource": [
        "arn:aws:dynamodb:region:account-number:table/course-notes",
        "arn:aws:s3:::course-notes-web",
        "arn:aws:s3:::course-notes-mp3/*"
      ]
    }
  ]
}
```

- **Explanation**:
  - **Allow**: Any DynamoDB or S3 action on the specified table and buckets.
  - **Deny**: Any DynamoDB or S3 action on all other resources not listed.
  - **Explicit Deny**: Takes precedence, ensuring access is limited only to the listed resources, even if other policies grant broader permissions.

---

### 3. Example: Identity-Based Policy

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iam:*LoginProfile",
        "iam:*AccessKey*",
        "iam:*SSHPublicKey*"
      ],
      "Resource": [
        "arn:aws:iam::account-id:user/${aws:username}"
      ]
    }
  ]
}
```

- **Explanation**:
  - Allows the user to manage their own password, access keys, and SSH public keys.
  - Uses wildcards (`*`) to cover all related actions.
  - The resource uses a variable to dynamically reference the current user's ARN.

---

### 4. Example: Cross-Account Resource-Based Policy

```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Sid": "AccountBAccess1",
    "Principal": { "AWS": "111122223333" },
    "Effect": "Allow",
    "Action": "s3:*",
    "Resource": [
      "arn:aws:s3:::DOC-EXAMPLE-BUCKET",
      "arn:aws:s3:::DOC-EXAMPLE-BUCKET/*"
    ]
  }
}
```

- **Explanation**:
  - Grants AWS account `111122223333` (Account B) permission to perform any S3 action on the specified bucket and its contents in Account A.
  - No specific IAM users/roles are mentioned—access is granted at the account level.
  - Account B must also have an identity-based policy to allow its users to access the bucket.

---

