# Amazon S3

## What is Amazon S3?

**Amazon Simple Storage Service (S3)** is AWS object storage used to store and retrieve data at scale.

Common examples include:

* Application files
* Backups
* Logs
* Static assets
* Build artifacts
* Static websites

### Basic Structure

```text
S3 Bucket
    ↓
Objects
    ↓
Files / Data
```

* **Bucket:** Container for storing objects.
* **Object:** The actual stored data/file along with its metadata.
* **Key:** The unique name/path used to identify an object inside a bucket.

---

## Why S3?

S3 is designed for:

* Scalability
* High durability
* High availability
* Security
* Cost optimization
* Large-scale object storage

S3 provides **99.999999999% (11 nines) durability** for objects.

An S3 bucket is created in a specific AWS Region, while objects can be accessed by authorized clients from anywhere with network connectivity.

---

# Bucket Versioning

S3 Versioning allows multiple versions of the same object to be stored.

### Why use it?

It helps protect against:

* Accidental overwrites
* Accidental deletions
* Unwanted changes

### Hands-on

I created an S3 bucket and uploaded:

```text
index.html
```

Then:

1. Enabled bucket versioning.
2. Modified `index.html`.
3. Uploaded the modified file again.
4. Opened the **Versions** section.
5. Verified that both versions were preserved.

### Concept

```text
index.html
     │
     ├── Version 1
     │
     └── Version 2
```

### Mental Model

> **Versioning = Object history and recovery**

It is similar conceptually to keeping previous versions of a file, although it is not a source-control system like Git.

---

# Static Website Hosting

S3 can host **static websites**, such as simple HTML/CSS/JavaScript sites.

### Basic Flow

```text
User
  ↓
S3 Website Endpoint
  ↓
index.html
  ↓
Web Page
```

Static website hosting is useful for simple websites, landing pages, documentation sites, and other static content.

---

## Hands-on: Static Website Hosting

I performed the following:

1. Created an S3 bucket.
2. Uploaded `index.html`.
3. Enabled static website hosting.
4. Configured the required public object access.
5. Added a bucket policy allowing `s3:GetObject`.
6. Accessed the website through the S3 website URL.

---

# Bucket Policy

An S3 Bucket Policy is a JSON-based **resource policy** that controls access to a bucket and its objects.

It can be used to:

* Allow access
* Deny access
* Restrict specific principals
* Restrict specific S3 actions
* Control access to specific resources

Example public-read policy used for the static website demonstration:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::<Bucket-Name>/*"
            ]
        }
    ]
}
```

### What does this mean?

```text
Principal: *
      ↓
Anyone

Action: s3:GetObject
      ↓
Can read/get objects

Resource: bucket/*
      ↓
Objects inside the bucket
```

So this policy makes the bucket's objects publicly readable.

### Security Warning

Public object access should only be used when the content is intentionally public.

Sensitive files such as:

* Database backups
* Credentials
* Private documents
* Internal application data

should not be made publicly readable.

---

# Storage Classes

S3 provides different storage classes based on access patterns and cost requirements.

| Storage Class           | Typical Use                                |
| ----------------------- | ------------------------------------------ |
| S3 Standard             | Frequently accessed data                   |
| S3 Intelligent-Tiering  | Data with changing/unknown access patterns |
| S3 Standard-IA          | Infrequently accessed data                 |
| S3 Glacier              | Archival data                              |
| S3 Glacier Deep Archive | Long-term archival                         |

### Mental Model

```text
Frequent access
      ↓
Higher storage cost
      ↓
Fast/easy access

Infrequent / archival
      ↓
Lower storage cost
      ↓
Retrieval considerations
```

The correct storage class depends on the application's access pattern, retrieval requirements, and cost constraints.

---

# Other Important S3 Concepts

## Scalability

S3 is designed to scale to very large amounts of object data without manually managing storage capacity.

## Object Size

An individual S3 object can be up to **5 TB**.

For large objects, S3 supports **multipart upload**, allowing the object to be uploaded in multiple parts.

## Durability

S3 is designed for extremely high object durability:

**99.999999999% (11 nines)**

## Security

S3 access can be controlled using mechanisms such as:

* IAM permissions
* Bucket policies
* Object/resource permissions
* Encryption
* S3 Block Public Access controls

Access should follow the principle of least privilege.

---

# S3 and DevOps

S3 is relevant to DevOps because it can be used for:

* Build artifacts
* Backups
* Logs
* Static assets
* Deployment packages
* Infrastructure-related files
* Static websites

Example:

```text
CI/CD Pipeline
      ↓
Build Application
      ↓
Artifact
      ↓
S3
      ↓
Storage / Distribution / Deployment
```

---

# Hands-on Summary

## Task 1: S3 Versioning

```text
Create Bucket
     ↓
Upload index.html
     ↓
Enable Versioning
     ↓
Modify index.html
     ↓
Upload again
     ↓
Verify both versions
```

**Result:** Multiple versions of the same object were preserved.

---

## Task 2: Static Website Hosting

```text
Create Bucket
     ↓
Upload index.html
     ↓
Enable Static Website Hosting
     ↓
Configure Public Access
     ↓
Add s3:GetObject Bucket Policy
     ↓
Open S3 Website URL
     ↓
Website loads
```

**Result:** Successfully accessed the uploaded HTML page through the S3 website endpoint.

---

# Key Takeaways

```text
S3
│
├── Object Storage
├── Buckets contain Objects
├── Versioning → Object history
├── Bucket Policy → Resource-based access control
├── Storage Classes → Cost/access optimization
├── Static Website Hosting
└── Large-scale durable storage
```

### Remember

> **S3 = Object Storage**

> **Versioning = Keep previous object versions**

> **Bucket Policy = Control access at the bucket/resource level**

> **Storage Class = Choose based on access pattern and cost**

> **Static Website Hosting = Serve static web content from S3**
