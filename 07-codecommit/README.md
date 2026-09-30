# AWS CodeCommit

## Overview

AWS CodeCommit is an AWS-managed source control service used to host Git repositories.

For learning purposes, it can be thought of as a GitHub-like repository service within AWS.

---

# AWS CI/CD Services

The video introduced four AWS services commonly associated with the CI/CD workflow:

| Service | Role |
|---|---|
| CodeCommit | Source code repository |
| CodePipeline | CI/CD orchestration |
| CodeBuild | Build and test |
| CodeDeploy | Application deployment |

### Basic Flow

```text
Developer
    ↓
CodeCommit
    ↓
CodePipeline
    ↓
CodeBuild
    ↓
CodeDeploy
    ↓
Application
````

### Mental Model

```text
CodeCommit  → Source
CodePipeline → Pipeline
CodeBuild   → Build
CodeDeploy  → Deploy
```

---

# Why CodeCommit?

The video explained that source code and infrastructure can be managed through AWS services rather than relying entirely on manual AWS Console operations.

A Git repository allows developers to:

* Store source code
* Track changes
* Clone repositories
* Commit changes
* Push changes

---

# Hands-on Lab

## 1. Create CodeCommit Repository

Opened AWS CodeCommit and created a repository:

```text
CC_repo
```

The repository can also be interacted with through Git.

---

## 2. Create IAM User

A dedicated IAM user was created for CodeCommit:

```text
CodeCommit-user
```

The policy attached during the lab was:

```text
AWSCodeCommitPowerUser
```

### Security Principle

Do not use the AWS Root account for CI/CD operations.

Use an IAM identity with the permissions required for the task.

---

# 3. Access Repository

The repository's HTTPS clone URL was copied from CodeCommit.

Git was already installed on my local machine.

The repository was cloned using:

```bash
git clone <CodeCommit-HTTPS-URL>
```

---

# 4. Authentication Problem

During the first attempt to interact with the repository through Git, I received:

```text
403
```

The problem was related to using credentials that were not the appropriate HTTPS Git credentials for AWS CodeCommit.

This was an important troubleshooting point.

---

# 5. Generate CodeCommit HTTPS Git Credentials

The required credentials were generated from the IAM user's security credentials.

Flow:

```text
IAM
 ↓
CodeCommit-user
 ↓
Security Credentials
 ↓
HTTPS Git credentials for AWS CodeCommit
 ↓
Generate credentials
```

These credentials were then used for Git authentication.

After configuring the correct credentials, the file was successfully pushed to the CodeCommit repository.

---

# Git Workflow

The basic workflow used in the lab was:

```text
CodeCommit Repository
        ↓
git clone
        ↓
Local Repository
        ↓
Create / Modify File
        ↓
git add
        ↓
git commit
        ↓
git push
        ↓
CodeCommit
```

Example:

```bash
git clone <repository-url>

cd <repository-directory>

git add .

git commit -m "Add test file"

git push
```

> Do not store actual AWS credentials in this README or commit them to Git.

---

# CodeCommit Console vs Git CLI

The CodeCommit console can be used for basic repository operations, including adding files through the UI.

Using Git from the CLI provides the normal Git workflow and allows multiple files and changes to be managed locally before pushing them to the repository.

---

# CodeCommit Limitations Discussed

The video discussed several limitations:

* AWS-specific service
* Fewer features discussed compared with alternatives
* Less integration with services outside AWS

Alternatives mentioned:

* GitHub
* GitLab

These platforms are commonly used for source control and integrate with a wider ecosystem of development and DevOps tools.

---

# Key Learning

The most important troubleshooting lesson from this lab was:

```text
Git clone / push
       ↓
403
       ↓
Check authentication
       ↓
Use correct CodeCommit HTTPS Git credentials
       ↓
Push succeeds
```

The important distinction is that the credentials used for AWS/IAM operations are not automatically the same thing as the HTTPS Git credentials required for CodeCommit Git authentication.

---

# Security Notes

* Do not use the AWS Root account for normal CI/CD operations.
* Use IAM identities with appropriate permissions.
* Follow least-privilege access where possible.
* Never commit Access Keys, Secret Keys, or CodeCommit Git passwords to GitHub.
* Remove or rotate credentials that are no longer required.

---

# D12 Hands-on Checklist

* [x] Created CodeCommit repository
* [x] Created IAM user
* [x] Attached CodeCommit permissions
* [x] Copied HTTPS clone URL
* [x] Cloned repository using Git
* [x] Encountered 403 authentication error
* [x] Generated CodeCommit HTTPS Git credentials
* [x] Authenticated successfully
* [x] Pushed file successfully

---

# Key Takeaways

```text
CodeCommit  = Source
CodePipeline = CI/CD orchestration
CodeBuild   = Build
CodeDeploy  = Deployment

403 during Git authentication
→ Check CodeCommit HTTPS Git credentials

Root account
→ Don't use for CI/CD

IAM
→ Use appropriate identity and permissions
