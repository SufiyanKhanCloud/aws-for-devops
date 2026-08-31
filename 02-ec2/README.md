# Amazon EC2

## What is EC2?

Amazon EC2 (Elastic Compute Cloud) provides resizable virtual servers in AWS.

It provides compute capacity that can be configured and managed according to workload requirements.

```text
AWS
 ↓
EC2
 ↓
Operating System
 ↓
Application
```

---

## Core Concepts

### AMI

An **Amazon Machine Image (AMI)** is a template used to launch an EC2 instance.

```text
AMI
 ↓
EC2 Instance
```

### Instance Type

Determines the compute characteristics available to the instance, such as CPU, memory, and networking capabilities.

### Key Pair

Used for authentication when connecting to Linux EC2 instances through SSH.

### Security Group

Acts as a virtual firewall controlling network traffic to and from the instance.

### EBS

Amazon Elastic Block Store provides persistent block storage commonly used with EC2.

### Public / Private IP

EC2 instances can communicate using private IP addresses within the VPC and may have public connectivity when appropriately configured.

---

## EC2 Lifecycle

```text
Launch
  ↓
Running
  ↓
Stop
  ↓
Stopped
  ↓
Start
  ↓
Running
```

Termination permanently removes the instance.

**Stop ≠ Terminate**

---

## SSH

Linux EC2 instances can commonly be accessed remotely using SSH.

```text
Local Machine
     ↓
SSH
     ↓
Security Group
     ↓
EC2
     ↓
Linux Shell
```

Typical structure:

```bash
ssh -i key.pem username@PUBLIC_IP
```

---

## IAM Role with EC2

An EC2 instance can use an IAM role to obtain AWS permissions without storing long-lived access keys on the server.

```text
EC2
 ↓
IAM Role
 ↓
AWS Permissions
 ↓
AWS Service
```

This is preferable to hardcoding AWS credentials.

---

## User Data

EC2 User Data can be used to bootstrap an instance during launch.

For example, it can install packages or configure services automatically.

This becomes especially useful with:

* Launch Templates
* Auto Scaling
* Infrastructure as Code

---

## Hands-on

During this learning session I:

* Created an EC2 instance.
* Connected to it through SSH.
* Worked with an Ubuntu Linux environment.
* Installed Java.
* Installed Jenkins.
* Accessed Jenkins through the EC2 instance.
* Configured the EC2 Security Group to allow the required traffic.

### Architecture

```text
Local Machine
     │
     │ SSH : 22
     ▼
Security Group
     │
     ▼
Ubuntu EC2
     │
     ├── Java
     │
     └── Jenkins : 8080
                    │
                    ▼
                 Browser
```

---

## Troubleshooting

When an SSH connection fails, check:

```text
Instance running?
       ↓
Correct public IP?
       ↓
Correct SSH username?
       ↓
Correct private key?
       ↓
Security Group allows TCP 22?
       ↓
Network connectivity?
       ↓
SSH service / OS health?
```

---

## DevOps Relevance

EC2 can host:

* Applications
* Jenkins
* Docker
* CI/CD runners
* Monitoring tools
* Internal services
* Web servers

However, EC2 should not automatically be chosen for every workload. Managed services, containers, and serverless alternatives may be more appropriate depending on the requirements.

---

## Key Takeaway

> **EC2 provides compute, while AMI defines the starting image, instance type defines the resources, EBS provides storage, Security Groups control network traffic, and IAM roles provide AWS permissions.**
