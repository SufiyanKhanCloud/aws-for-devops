# AWS CloudFormation

## What is CloudFormation?

AWS CloudFormation is an Infrastructure as Code (IaC) service used to define and provision AWS infrastructure through templates.

Instead of manually creating resources through the AWS Console, we define the desired infrastructure in a template and CloudFormation creates and manages the resources as a stack.

### Mental Model

```text
CloudFormation Template
        ↓
   CloudFormation
        ↓
    AWS Resources
````

CloudFormation acts as the layer between the infrastructure template and the AWS resources being created.

---

## CloudFormation Templates

CloudFormation templates can be written in:

* YAML
* JSON

For this hands-on, YAML was used.

YAML is easier to read and maintain compared with JSON, especially as templates become larger.

---

## Declarative Infrastructure

CloudFormation is **declarative**.

This means we describe:

> **WHAT infrastructure/state we want**

rather than writing every step for:

> **HOW AWS should create it**

For example, instead of manually creating an S3 bucket through multiple console steps, the template defines the desired S3 resource and CloudFormation handles the provisioning.

### Mental Model

```text
Declarative

"I want this infrastructure"

        ↓

CloudFormation

        ↓

AWS creates/manages it
```

---

## CloudFormation Stacks

CloudFormation manages resources through **stacks**.

A stack is created from a CloudFormation template and contains the AWS resources defined by that template.

Basic flow:

```text
Template
   ↓
Create Stack
   ↓
CloudFormation
   ↓
AWS Resources
```

---

# CloudFormation vs AWS CLI

Both can be used to interact with AWS, but they solve different problems.

### AWS CLI

Useful for:

* Quick commands
* Ad-hoc operations
* Resource inspection
* Short tasks
* Scripting

Example:

```bash
aws s3 ls
```

### CloudFormation

Useful for:

* Infrastructure as Code
* Repeatable infrastructure
* Managing multiple related resources
* Version-controlled infrastructure
* Infrastructure lifecycle management

### Mental Model

```text
AWS CLI
→ "Do this operation"

CloudFormation
→ "This is the infrastructure I want"
```

---

# Drift Detection

One of the important CloudFormation features demonstrated in this lab was **Drift Detection**.

Drift occurs when the actual AWS resource configuration becomes different from the configuration defined by the CloudFormation template.

### Mental Model

```text
CloudFormation Template
        ↓
Desired State

        VS

Actual AWS Resource
        ↓
Actual State
```

If the two no longer match, CloudFormation can detect the drift.

---

# Hands-on Lab 1: S3 Bucket + Resource Deletion

## Objective

Create an S3 bucket through CloudFormation and test whether CloudFormation can detect a manual deletion.

### Steps

1. Opened AWS CloudFormation.
2. Started creating a stack.
3. Used the Infrastructure Composer option.
4. Created an S3 resource through CloudFormation.
5. Created the stack.
6. Verified that the S3 bucket was created.
7. Manually deleted the S3 bucket outside CloudFormation.
8. Returned to the CloudFormation stack.
9. Ran Drift Detection.

### Result

CloudFormation detected that the S3 bucket created by the stack was no longer present.

### What I learned

CloudFormation can detect when the actual infrastructure no longer matches the expected stack state.

---

# Hands-on Lab 2: S3 Versioning Drift

## Objective

Test configuration drift by changing an S3 bucket configuration manually.

### Steps

1. Created an S3 bucket using CloudFormation.
2. Enabled S3 Versioning through the CloudFormation configuration.
3. Created the stack.
4. Verified the bucket.
5. Manually disabled Versioning through the AWS Console.
6. Returned to CloudFormation.
7. Ran Drift Detection.

### Result

CloudFormation detected that the S3 bucket's actual configuration had changed from the configuration defined by the stack.

### Mental Model

```text
CloudFormation
Versioning = ON
      ↓
Manual UI change
      ↓
Versioning = OFF
      ↓
Drift Detection
      ↓
DRIFT DETECTED
```

---

# Creating a CloudFormation Template Locally

For the second hands-on approach, a CloudFormation template was created as a YAML file on the local machine.

The template was then uploaded during stack creation.

### Flow

```text
Local YAML Template
        ↓
CloudFormation
        ↓
Create Stack
        ↓
S3 Bucket
```

This demonstrates that infrastructure definitions can be stored as files rather than being created only through the AWS Console.

---

# VS Code Extensions

The following VS Code extensions were used/recommended during the learning process:

* YAML
* AWS Toolkit

These provide useful support when working with AWS templates and YAML files.

---

# CloudFormation vs Terraform

### CloudFormation

* AWS-native IaC tool
* Designed specifically for AWS infrastructure
* Deep integration with AWS services

### Terraform

* Infrastructure as Code tool
* Supports multiple infrastructure providers
* Useful for multi-cloud and hybrid environments

### Basic Mental Model

```text
CloudFormation
        ↓
     AWS-focused


Terraform
        ↓
Multi-provider / Multi-cloud
```

The choice depends on the infrastructure and organizational requirements.

---

# Why CloudFormation Matters for DevOps

CloudFormation introduces an important DevOps concept:

> **Infrastructure can be defined as code instead of being created manually.**

This makes infrastructure easier to:

* Repeat
* Review
* Version
* Automate
* Manage consistently

It also helps identify manual changes through drift detection.

---

# Key Takeaways

```text
CloudFormation = AWS Infrastructure as Code

YAML / JSON
     ↓
CloudFormation Template
     ↓
CloudFormation Stack
     ↓
AWS Resources
```

### Remember

* CloudFormation is declarative.
* Templates can be written in YAML or JSON.
* Resources are managed through stacks.
* Infrastructure definitions can be version controlled.
* Drift Detection identifies differences between the expected stack configuration and the actual resource state.
* CLI is useful for quick operations.
* CloudFormation is useful for repeatable infrastructure management.
* Terraform is a multi-provider IaC alternative.

---

# Hands-on Status

* [x] Created CloudFormation stack
* [x] Used Infrastructure Composer
* [x] Created S3 bucket using CloudFormation
* [x] Created CloudFormation template using YAML
* [x] Uploaded YAML template to CloudFormation
* [x] Tested resource deletion drift
* [x] Tested S3 Versioning configuration drift
* [x] Used YAML extension in VS Code
* [x] Installed/used AWS Toolkit
