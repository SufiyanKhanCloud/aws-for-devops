# AWS IAM

## What is IAM?

**AWS Identity and Access Management (IAM)** controls who can access AWS resources and what actions they are allowed to perform.

IAM is primarily about **authentication and authorization**.

```text
Who are you?
     ↓
Authentication

What are you allowed to do?
     ↓
Authorization
```

---

## IAM Users

An IAM user represents an identity that can be given AWS permissions.

A user can have permissions through policies.

Example:

```text
IAM User
   ↓
IAM Policy
   ↓
AWS Permissions
```

For example, a policy could allow a user to read objects from a specific S3 bucket.

---

## IAM Groups

An IAM group is a collection of IAM users.

Groups make it easier to assign common permissions to multiple users.

```text
Developers Group
│
├── User A
├── User B
└── User C
```

A policy can be attached to the group instead of repeatedly attaching the same policy to every user.

---

## IAM Roles

An IAM role provides permissions that can be assumed by trusted entities.

Roles are especially important for AWS services.

Example:

```text
EC2
 ↓
IAM Role
 ↓
Permissions
 ↓
S3
```

This is preferable to storing long-lived AWS access keys on the EC2 instance.

### DevOps importance

IAM roles are heavily used with:

* EC2
* ECS
* Lambda
* CI/CD
* Terraform
* Other AWS services

---

## IAM Policies

Policies define what actions are allowed or denied.

A policy can specify:

* Effect
* Action
* Resource
* Conditions

Conceptually:

```text
Who?
 ↓
Can perform what action?
 ↓
On which resource?
 ↓
Under what conditions?
```

Example:

```text
Effect: Allow
Action: s3:GetObject
Resource: specific S3 objects
```

---

## Users vs Groups vs Roles

| Component | Purpose                                                                   |
| --------- | ------------------------------------------------------------------------- |
| User      | Identity representing a person/application user                           |
| Group     | Organizes users and applies common permissions                            |
| Role      | Temporary/assumable identity used by AWS services, users, workloads, etc. |
| Policy    | Defines permissions                                                       |

---

## Security Principle

### Least Privilege

Give an identity only the permissions it actually needs.

Avoid:

```text
AdministratorAccess
```

when the workload only requires:

```text
s3:GetObject
```

The goal is to minimize unnecessary permissions.

---

## DevOps Relevance

IAM affects almost every AWS deployment.

Examples:

```text
EC2 → IAM Role → S3

Lambda → IAM Role → DynamoDB

CI/CD → IAM Role → AWS Deployment

Terraform → IAM Identity → AWS APIs
```

Understanding IAM is therefore fundamental to secure AWS automation.

---

## Key Takeaways

* IAM controls AWS access and permissions.
* Users represent identities.
* Groups organize users.
* Roles provide assumable permissions.
* Policies define permissions.
* Prefer roles over long-lived credentials for workloads.
* Follow least privilege.
* IAM permissions and network security are different concepts.

### Mental Model

```text
IAM = "Who can do what on which AWS resource?"
```
