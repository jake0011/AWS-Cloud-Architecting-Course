# AWS Academy Cloud Architecting – Encrypting Data at Rest Using AWS Encryption Options

## Introduction
In this lab, you will explore how AWS helps secure data at rest using different encryption options.  
You will examine encryption in Amazon S3 and Amazon EBS, use the AWS Key Management Service (AWS KMS) to create and manage encryption keys, and review AWS CloudTrail logs to observe encryption-related events.

---

## Learning Objectives
After completing this lab, you will be able to:

- Review S3 default encryption (SSE-S3)
- Create and use a Customer Managed Key (CMK) in AWS KMS
- Encrypt an EBS volume and attach it to an EC2 instance
- Disable and re-enable the CMK to observe access impact
- Monitor key usage through AWS CloudTrail
- Review automatic key rotation

---

## Prerequisites
Before starting this lab, ensure you:

- Have access to an AWS Academy Learner Lab environment
- Are familiar with the AWS Management Console
- Understand basic AWS services (EC2, S3, IAM)

---

## Lab Overview
This lab consists of the following tasks:

1. **Review S3 Default Encryption**
2. **Create a Customer Managed Key (CMK)**
3. **Encrypt and Attach an EBS Volume**
4. **Test Key Disable/Enable Functionality**
5. **Monitor KMS Activity Using CloudTrail**
6. **Enable Key Rotation**

You will begin by verifying encryption settings for an existing S3 bucket, then create a CMK in AWS KMS, use that key to encrypt data in Amazon EBS, and review CloudTrail logs for key usage. Finally, you will enable automatic key rotation.

---

## Estimated Time
**45–60 minutes**

---

## AWS Services Used
- **Amazon S3**
- **Amazon Elastic Block Store (EBS)**
- **AWS Key Management Service (KMS)**
- **AWS CloudTrail**
- **AWS Identity and Access Management (IAM)**

---

## Lab Steps

### Task 1: Review S3 Default Encryption
1. Open the **Amazon S3** console.
2. Navigate to the provided S3 bucket.
3. Review **Default Encryption** settings under the **Properties** tab.
4. Confirm that **SSE-S3 (AES-256)** is enabled.

---

### Task 2: Create a Customer Managed Key (CMK)
1. Open the **AWS KMS** console.
2. Choose **Create key** → **Symmetric**.
3. Provide an alias, such as `LabCMK`.
4. Add a description and set key administrators and users.
5. Complete the creation process.
6. Copy the **Key ARN** for future use.

---

### Task 3: Encrypt and Attach an EBS Volume
1. Open the **EC2** console.
2. Choose **Volumes → Create Volume**.
3. Select the same Availability Zone as your instance.
4. Choose **Encrypt this volume**, and select your newly created CMK.
5. Attach the volume to your EC2 instance.

---

### Task 4: Test Key Disable/Enable Functionality
1. In the **KMS** console, select your CMK.
2. Choose **Disable key**.
3. Attempt to access the encrypted EBS volume — you should receive an access denied error.
4. Re-enable the key and verify that access is restored.

---

### Task 5: Monitor KMS Activity Using CloudTrail
1. Open the **CloudTrail** console.
2. Navigate to **Event history**.
3. Filter by **Event source: kms.amazonaws.com**.
4. Review encryption and decryption activity logs.

---

### Task 6: Enable Key Rotation
1. In the **KMS** console, select your CMK.
2. Choose **Key rotation** → **Enable automatic key rotation**.
3. Review key rotation policy and confirm.

---

## Conclusion
You have successfully:
- Reviewed S3 default encryption.
- Created and managed an AWS KMS Customer Managed Key.
- Encrypted an EBS volume using KMS.
- Tested key disable and enable states.
- Monitored key usage with AWS CloudTrail.
- Enabled automatic key rotation.

These steps demonstrate how AWS provides comprehensive options to secure data at rest using encryption.

---

## End of Lab
