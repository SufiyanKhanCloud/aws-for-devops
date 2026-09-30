# AWS CodePipeline

## Overview

AWS CodePipeline is a managed CI/CD orchestration service.

It coordinates different stages of a software delivery workflow, connecting source, build, test, and deployment services.

---

# Jenkins CI/CD Flow

The video first explained a typical Jenkins-based CI/CD workflow.

```text
Developer
    ↓
Git Repository
    ↓
Jenkins
    ↓
Code Checkout
    ↓
Build
    ↓
Testing
    ↓
Static Code Analysis
    ↓
Containerization
    ↓
Deployment
    ↓
Application
````

Jenkins acts as the pipeline/orchestration platform and can integrate with many different tools for individual stages.

---

# AWS CI/CD Flow

The AWS equivalent discussed in the video uses multiple managed services:

```text
Source Repository
       ↓
CodePipeline
       ↓
CodeBuild
       ↓
CodeDeploy
       ↓
Application
```

The source repository can be AWS CodeCommit or an external repository such as GitHub, depending on the pipeline setup.

### AWS Service Roles

| Service      | Role                   |
| ------------ | ---------------------- |
| CodeCommit   | Source code repository |
| CodePipeline | Pipeline orchestration |
| CodeBuild    | Build and CI tasks     |
| CodeDeploy   | Deployment             |

### Mental Model

```text
CodeCommit  → Source
CodePipeline → Orchestrate
CodeBuild   → Build/Test
CodeDeploy  → Deploy
```

---

# Jenkins vs AWS CodePipeline

## Jenkins

Jenkins is an open-source automation server commonly used for CI/CD.

The video highlighted:

* Open-source
* Large plugin ecosystem
* Extensive integrations
* Multi-cloud flexibility
* Greater control over infrastructure
* Requires infrastructure management

A Jenkins environment may require management of:

* Jenkins controller
* Build/worker nodes
* Plugins
* Scaling
* Updates
* Infrastructure

---

## AWS CodePipeline

CodePipeline is an AWS-managed CI/CD orchestration service.

The video highlighted:

* Managed by AWS
* Less infrastructure management
* Integrates with AWS services
* Scalable managed service
* Pay-as-you-go pricing
* More AWS-focused

The trade-off is that AWS-managed CI/CD services can create stronger dependency on the AWS ecosystem compared with a more portable Jenkins setup.

---

# Management Difference

### Jenkins

```text
Your responsibility
      ↓
Jenkins infrastructure
      ↓
Jenkins + plugins + workers
      ↓
CI/CD pipeline
```

### AWS CodePipeline

```text
AWS-managed service
      ↓
CodePipeline
      ↓
CodeBuild / CodeDeploy / other services
```

The main difference discussed in the video is **management overhead versus flexibility/control**.

---

# CI/CD Stages

A typical CI pipeline can contain stages such as:

```text
Checkout
   ↓
Build
   ↓
Unit Tests
   ↓
Static Analysis
   ↓
Container Build
   ↓
Security/Scanning
   ↓
Artifact
```

CodePipeline can orchestrate these stages while services such as CodeBuild perform the actual build and test work.

---

# Vendor Lock-in

Jenkins can run in different environments such as:

* AWS
* Azure
* Other cloud environments
* On-premises

AWS CodePipeline is primarily designed around the AWS ecosystem.

This creates a trade-off:

```text
Jenkins
→ More portability/flexibility
→ More management

AWS CodePipeline
→ AWS integration + managed experience
→ Less infrastructure management
→ Greater AWS dependency
```

---

# D13 Status

**Theory completed:** ✅

Covered:

* [x] Jenkins CI/CD flow
* [x] AWS CI/CD flow
* [x] CodePipeline role
* [x] CodeCommit role
* [x] CodeBuild role
* [x] CodeDeploy role
* [x] Jenkins vs CodePipeline
* [x] Management overhead
* [x] Multi-cloud vs AWS-focused approach
* [x] Vendor lock-in concept
