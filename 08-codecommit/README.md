# AWS CodeCommit

## Overview

AWS CodeCommit is a managed Git-based source control service in AWS.

It can be used to host private Git repositories and integrates with other AWS CI/CD services.

For this video, the focus was specifically on **CodeCommit** and how it fits into the AWS CI/CD ecosystem.

---

# AWS CI/CD Services

AWS provides several managed services that can be combined to build a CI/CD workflow:

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

### Services

| Service      | Purpose                      |
| ------------ | ---------------------------- |
| CodeCommit   | Source code / Git repository |
| CodePipeline | CI/CD workflow orchestration |
| CodeBuild    | Build and test application   |
| CodeDeploy   | Deploy application           |

### Simple Mental Model

```text
CodeCommit
→ Where is my code?

CodePipeline
→ How does the workflow move?

CodeBuild
→ Build/test the application

CodeDeploy
→ Deploy the application
```

---

# CodeCommit

CodeCommit provides Git repositories managed by AWS.

It can be thought of as an AWS-managed alternative for hosting Git repositories.

Repositories can be accessed through:

* AWS Console
* Git CLI
* HTTPS
* SSH

---

# Hands-on: Create CodeCommit Repository

## Step 1: Create Repository

Created a CodeCommit repository through the AWS Console.

Repository name:

```text
CC_repo
```

The repository can be used to store Git-based source code.

---

# Adding Files

Files can be added through the AWS Console UI.

The UI can be useful for simple file operations.

For adding multiple files or working with a normal development workflow, Git from the terminal is more practical.

### Git workflow

```text
Local Repository
      ↓
    git add
      ↓
   git commit
      ↓
    git push
      ↓
CodeCommit Repository
```

---

# IAM Access

The video demonstrated that the AWS **root account should not be used for normal CI/CD operations**.

Instead, an IAM identity should be used with appropriate permissions.

For this lab, an IAM user was created:

```text
CodeCommit-user
```

The user was given the required CodeCommit permissions.

The policy used during the lab was:

```text
AWSCodeCommitPowerUser
```

### Important Security Principle

Use appropriate IAM permissions instead of using the AWS root account for everyday operations.

---

# CodeCommit Region

After creating the repository and IAM user, the AWS Console region was set to:

```text
us-east-1
```

The CodeCommit repository was then accessed from that region.

---

# Clone CodeCommit Repository

The HTTPS clone URL was copied from the CodeCommit repository.

Git was already installed on the local machine.

The repository was cloned using:

```bash
git clone <CodeCommit-HTTPS-URL>
```

Git then requested credentials.

---

# 403 Error During Git Clone/Push

During the hands-on, the first Git authentication attempt resulted in:

```text
403 Forbidden
```

The issue was related to using the wrong credentials for CodeCommit Git authentication.

The credentials of the IAM user itself are not automatically the same thing as the **HTTPS Git credentials required by CodeCommit**.

---

# Generating HTTPS Git Credentials

The issue was resolved by going to:

```text
IAM
  ↓
CodeCommit IAM User
  ↓
Security Credentials
  ↓
HTTPS Git credentials for AWS CodeCommit
  ↓
Generate credentials
```

The generated CodeCommit Git credentials were then used for Git authentication.

After using the correct credentials, the Git operation succeeded.

---

# Git Push

After successfully authenticating with CodeCommit, a file was pushed to the repository.

Basic workflow:

```bash
git add .
git commit -m "Add file"
git push
```

The file was successfully pushed to the CodeCommit repository.

---

# What the 403 Error Taught Me

The important lesson from the failure was:

```text
AWS IAM identity
        ≠
CodeCommit HTTPS Git credentials
```

The correct credentials/mechanism must be used for the Git authentication method being used.

This was useful because the problem was not Git itself. The repository was reachable, but authentication was incorrect.

---

# CodeCommit vs GitHub / GitLab

CodeCommit provides Git repository functionality, but the video highlighted some disadvantages compared with platforms such as GitHub and GitLab.

### CodeCommit

* AWS-native
* Managed by AWS
* Integrates with AWS services
* Useful for AWS-focused environments

### GitHub / GitLab

* Richer developer features
* Broader integrations
* More extensive collaboration capabilities
* Commonly used outside AWS-specific environments

The video recommended continuing to learn and use platforms such as GitHub/GitLab rather than relying only on CodeCommit.

---

# Limitations Discussed

The video highlighted these disadvantages of CodeCommit:

### 1. Fewer features

CodeCommit has fewer developer/collaboration features compared with feature-rich platforms such as GitHub and GitLab.

### 2. AWS-focused

CodeCommit is tightly connected to the AWS ecosystem.

### 3. Fewer external integrations

It has less integration with services outside the AWS ecosystem compared with mainstream Git platforms.

---

# DevOps Relevance

Understanding CodeCommit is useful for understanding how AWS can provide a complete managed CI/CD ecosystem:

```text
CodeCommit
     ↓
CodePipeline
     ↓
CodeBuild
     ↓
CodeDeploy
```

However, this does not replace the importance of understanding mainstream Git platforms such as GitHub and GitLab.

---

# Key Takeaways

```text
CodeCommit
→ Managed Git repository in AWS

CodePipeline
→ CI/CD workflow orchestration

CodeBuild
→ Build and test

CodeDeploy
→ Deployment
```

### Remember

> CodeCommit = Source Code

> CodePipeline = Pipeline orchestration

> CodeBuild = Build

> CodeDeploy = Deployment

> Use IAM identities and appropriate permissions instead of the root account.

> CodeCommit Git authentication may require dedicated HTTPS Git credentials.

> A 403 during Git authentication can be a credentials/authentication problem rather than a repository or Git problem.

---

# Hands-on Status

* [x] Created CodeCommit repository
* [x] Created IAM user for CodeCommit
* [x] Assigned CodeCommit permissions
* [x] Set region to `us-east-1`
* [x] Obtained CodeCommit HTTPS clone URL
* [x] Cloned repository using Git
* [x] Encountered `403` authentication error
* [x] Generated HTTPS Git credentials for CodeCommit
* [x] Successfully authenticated
* [x] Pushed a file to CodeCommit
* [x] Compared CodeCommit with GitHub/GitLab
