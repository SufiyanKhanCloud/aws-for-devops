# AWS CLI

## What is AWS CLI?

AWS Command Line Interface (AWS CLI) is a command-line tool used to interact with AWS services from a terminal.

Instead of performing every operation through the AWS Management Console, AWS CLI allows AWS resources and services to be managed using commands.

### Mental Model

```text
Terminal
   ↓
AWS CLI
   ↓
AWS API
   ↓
AWS Resource
````

---

## Why Use AWS CLI?

The AWS Management Console is useful for learning and manual operations, but repetitive tasks can become slow when performed through the UI.

AWS CLI helps with:

* Faster repetitive operations
* Automation
* Scripting
* Quick resource inspection
* DevOps workflows
* CI/CD integration

Example:

```bash
aws s3 ls
```

Instead of opening the S3 console and manually checking the bucket list, the command retrieves the bucket list directly from the terminal.

---

# AWS Infrastructure Automation Tools

AWS infrastructure can be managed using different approaches.

| Tool           | Main Purpose                                      |
| -------------- | ------------------------------------------------- |
| AWS Console    | Manual management                                 |
| AWS CLI        | Command-line / quick operations                   |
| Terraform      | Infrastructure as Code                            |
| CloudFormation | AWS Infrastructure as Code                        |
| AWS CDK        | Define infrastructure using programming languages |

### Mental Model

```text
Console
→ Manual interaction

CLI
→ Commands and automation scripts

Terraform / CloudFormation / CDK
→ Repeatable infrastructure management
```

AWS CLI is useful for quick operations and scripting, while Infrastructure as Code tools are more appropriate when infrastructure needs to be defined, reviewed, version-controlled, and reproduced consistently.

---

# AWS CLI Installation

I installed the AWS CLI on my local machine using the installation instructions provided by AWS.

After installation, the CLI can be verified with:

```bash
aws --version
```

---

# AWS CLI Configuration

The AWS CLI can be configured using:

```bash
aws configure
```

The configuration asks for:

```text
AWS Access Key ID
AWS Secret Access Key
Default region name
Default output format
```

For this lab I configured:

```text
Default Region:
us-east-1

Output Format:
json
```

### Security

Access keys are credentials and must be protected.

**Never commit credentials to GitHub.**

Do not put actual:

```text
Access Key ID
Secret Access Key
```

inside:

* GitHub repositories
* README files
* screenshots
* scripts
* public documentation

Use appropriate IAM permissions and the principle of least privilege.

AWS also supports other credential mechanisms, including IAM roles and IAM Identity Center, which can avoid storing long-term credentials in local workflows.

---

# Hands-on

## 1. List S3 Buckets

After configuring the AWS CLI, I used:

```bash
aws s3 ls
```

This command lists the S3 buckets accessible through the configured AWS identity.

### Flow

```text
Terminal
   ↓
aws s3 ls
   ↓
AWS API
   ↓
S3
   ↓
Bucket List
```

---

# 2. Create an EC2 Instance Using AWS CLI

I also used AWS CLI from the terminal to create an EC2 instance.

This demonstrated that AWS resources do not need to be created exclusively through the AWS Management Console.

The general idea is:

```text
Terminal
   ↓
AWS CLI command
   ↓
EC2 API
   ↓
EC2 Instance
```

The exact EC2 command depends on parameters such as:

* AMI ID
* Instance type
* Key pair
* Security group
* Subnet
* IAM role/profile
* Storage configuration

---

# AWS CLI Documentation

A major skill is learning how to find commands from AWS documentation instead of memorizing every command.

For a service:

```text
aws <service> <operation>
```

For example:

```bash
aws s3 ls
```

The AWS CLI documentation provides the available operations and their parameters.

**Important DevOps skill:**

> Know how to find and construct the command rather than memorizing hundreds of commands.

---

# CLI vs Infrastructure as Code

## AWS CLI

Best suited for:

* Quick operations
* Ad-hoc tasks
* Resource inspection
* Troubleshooting
* Small scripts
* Automation inside shell scripts

Example:

```bash
aws s3 ls
```

## Terraform / CloudFormation / CDK

Better suited when infrastructure needs to be:

* Repeatable
* Version controlled
* Reviewed
* Recreated consistently
* Managed as code
* Used across environments

### Mental Model

```text
AWS CLI
    ↓
"Do this operation now"

IaC
    ↓
"This is how my infrastructure should be"
```

This distinction becomes particularly important for DevOps and Cloud Engineering.

---

# DevOps Relevance

AWS CLI is important for DevOps because it can be used inside:

* Shell scripts
* CI/CD pipelines
* Deployment automation
* Troubleshooting workflows
* Infrastructure scripts
* Operational tooling

Example:

```text
Git Push
   ↓
CI/CD Pipeline
   ↓
AWS CLI
   ↓
AWS API
   ↓
AWS Resource
```

---

# Hands-on Summary

### Completed

* [x] Installed AWS CLI
* [x] Configured AWS CLI
* [x] Configured default region: `us-east-1`
* [x] Configured output format: `json`
* [x] Listed S3 buckets using `aws s3 ls`
* [x] Created an EC2 instance using AWS CLI

---

# Key Takeaways

```text
AWS CLI
   ↓
Command-line access to AWS
   ↓
Fast + scriptable + automatable
```
