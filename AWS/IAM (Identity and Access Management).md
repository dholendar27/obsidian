---
date created: 2025-10-27 20:37
date updated: 2025-10-28 09:57
---

AWS IAM is an security service provided by the amazon web services that helps to control access to our AWS resources.

> [!note] IAM
> IAM lets you decide who (users, groups, or applications) can do what (specific actions like read/write/delete) and where (which AWS resources).

### Key Components of IAM

| Component                     | Description                                                                                                                    |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| **Users**                     | Individual people or applications that need access to AWS resources. Each user has unique credentials (password, access keys). |
| **Groups**                    | A collection of IAM users. Permissions assigned to a group are inherited by all its members.                                   |
| **Roles**                     | Similar to a user but without credentials. Roles are assumed temporarily by users, applications, or AWS services.              |
| **Policies**                  | JSON documents that define permissions (what actions are allowed or denied on which resources).                                |
| **Identity Providers (IdPs)** | Allow external users (e.g., from Google, Microsoft AD, or SSO systems) to access AWS resources securely.                       |

### Uses of IAM

IAM is used for **managing access control and security** across AWS environments. Here are the main use cases:

##### 1. User Authentication and Authorization

- Create and manage user identities.
- Assign permissions for specific AWS services (like S3, EC2, Lambda, etc.).
- Example: Give a developer “read-only” access to S3 buckets but full access to EC2.

##### 2. Role-Based Access Control (RBAC)

- Use **roles** for EC2 instances, Lambda functions, or ECS tasks to securely access AWS resources **without storing access keys**.
- Example: An EC2 instance can assume a role that lets it read data from S3 but not delete it.

##### 3. Multi-Factor Authentication (MFA)

- Enforce additional authentication (e.g., using an OTP or hardware token).
- Prevents unauthorized access even if passwords are stolen.

##### 4. Temporary Access (STS & Federated Access)

- IAM integrates with **AWS STS (Security Token Service)** to provide temporary credentials.
- External users (like contractors or applications) can be given time-limited access.

##### 5. Centralized Permissions Management

- Manage permissions for multiple AWS accounts via **AWS Organizations** and **IAM Identity Center (AWS SSO)**.

##### 6. Fine-Grained Access Control

- Apply precise permissions (e.g., allow `s3:GetObject` but deny `s3:DeleteObject` on a specific bucket).

### Why IAM is Important

IAM is **critical for AWS security** and proper governance. Here’s why:

##### 1. Security Foundation

- IAM is at the **core of AWS security**.
- It ensures only **authorized users** and **services** can access sensitive resources.

##### 2. Least Privilege Principle

- Assign the **minimum necessary permissions** to users or roles.
- Reduces risk of accidental or malicious actions.

##### 3. Auditing and Compliance

- AWS IAM integrates with **CloudTrail** to record all authentication and authorization activity.
- Essential for compliance (e.g., HIPAA, PCI-DSS, GDPR).

##### 4. Scalability and Manageability

- Easily manage access for hundreds or thousands of users across multiple services and accounts.
- Use groups, roles, and policies for consistency.

##### 5. Secure Automation

- Automate AWS operations (through SDKs or CLI) using **roles** instead of embedding access keys in code.

##### 6. Cross-Account and Federated Access

- Securely share resources between AWS accounts or external identity providers.

---

## IAM User

**IAM Users** (Identity and Access Management Users) are individual identities that we create in **AWS Identity and Access Management (IAM)** so that people or applications can access your AWS resources securely.

> [!note] IAM User
> An IAM User represents a person or application that interacts with AWS resources.
> Each user has its own credentials (like username, password, and/or access keys).

### Types of IAM User Credentials

1. **Console access:**
   - Username + password
   - Used to sign in to the AWS Management Console

2. **Programmatic access:**
   - Access key ID + secret access key
   - Used to access AWS via the **CLI**, **SDK**, or **API**

### Permissions

- By default, IAM users have **no permissions**.
- You must **attach IAM policies** (JSON documents) that define **what actions** the user can perform and **on which resources**.
- Permissions can be attached:
  - Directly to the user
  - Through **groups**
  - Via **roles** (for temporary access)

---

## IAM Policies

The IAM Policies are used to define what action a user, group or role can perform on AWS resource.

### What is IAM policy?

- An IAM policy is a JSON Document that explicitly grants or denies permissions to AWS resources.
- Policies control **who can do what on which resource**
- without policy, user cannot access any AWS resources.

### Components of IAM policy

A typical IAM policy has:

1. **Version**
   - Defines the policy language version.
   - Example: `"Version": "2012-10-17"`
2. **Statement** (mandatory)
   - Contains one or more statements defining permissions.
   - Each statement has:
     - `Effect`: `Allow` or `Deny`
     - `Action`: What actions are allowed or denied (e.g., `s3:ListBucket`)
     - `Resource`: Which resource(s) the action applies to (e.g., `arn:aws:s3:::my-bucket`)
     - `Condition` (optional): Adds restrictions (like time, IP address, etc.)

### Types of IAM Policies

1. **Managed Policies**
   - **AWS Managed Policies:** Created and maintained by AWS.
   - **Customer Managed Policies:** Created and maintained by your account.
2. **Inline Policies**
   - Embedded directly into a **user, group, or role**.
   - Tightly coupled with that identity.

### Policy Evaluation Logic

- **Default Deny**
  - AWS is super secure by default.
  - If there is **no policy**, a user **cannot do anything**.
- **Explicit Allow**
  - If a policy **explicitly allows** an action, the user can perform it.
- **Explicit Deny**
  - If a policy **explicitly denies** an action, it **overrides any Allow**.
  - Even if some other policy says “Allow,” Deny **always wins**.
- **No Matching Policy = Deny**
  - If a requested action **doesn’t match any policy**, it’s denied by default.

---

## IAM Roles

An **IAM role** is a set of permissions that define what actions are allowed or denied on specific resources within your AWS environment. Unlike users, IAM roles are not associated with a specific person or identity but are intended to be assumed by authorized entities (like users, applications, or AWS services) to perform certain actions.

An **IAM role** is a set of permissions that you can assign to a user, a group of users, or even a service. The role defines what actions the person or service can perform and which resources they can access.

### Why do we use IAM Roles?

The main reason we use IAM roles is **security** and **access control**. With IAM roles, you can:
1. **Restrict access** to resources: You only allow certain users or services to access specific resources (e.g., S3 buckets, EC2 instances).
2. **Grant temporary access**: You can give permissions for a limited time. This is useful for temporary tasks, like a support technician needing access for a short period.
3. **Allow cross-service access**: Sometimes, one service in the cloud needs to interact with another service. IAM roles allow this securely.
### How Do IAM Roles Work?

1. **Create the Role**: First, you create a role in IAM. This role will have specific permissions attached to it, such as reading from an S3 bucket, writing to a database, or launching an EC2 instance.
2. **Assign the Role**: You then assign this role to a user, an EC2 instance, or a service like Lambda. This grants them the permissions associated with the role.
3. **Role Assumption**: When a user or service needs to perform an action, they “assume” the role. This means they take on the permissions of that role temporarily. For example, an EC2 instance can assume a role to access an S3 bucket to upload files.
### Types of IAM Roles

- **User Role**: This role is assigned to an individual user, giving them access to specific resources.
- **Service Role**: A service role is given to a cloud service (e.g., EC2, Lambda) to allow the service to perform actions on your behalf.
- **Assumed Role**: Sometimes a role is assumed by a user or service temporarily, for example, an external service needing temporary access to your resources.

### Simple Example

Imagine you have a team of developers working in AWS. You don’t want all of them to have access to everything (like deleting databases or launching EC2 instances). So, you create an **IAM role** called **"DeveloperRole"** that gives access to the resources they need (like read-only access to S3 and permissions to work with certain EC2 instances). Then, you assign that role to the developer users.
If a service like **Lambda** needs to access your S3 bucket to read data, you create a service role that allows Lambda to access S3. When Lambda runs, it "assumes" that role and gets the necessary permissions.