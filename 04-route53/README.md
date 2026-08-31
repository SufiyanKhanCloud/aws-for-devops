# Amazon Route 53

## What is Route 53?

Amazon Route 53 is AWS's highly available and scalable **DNS (Domain Name System) service**.

DNS allows users to access applications using human-readable domain names instead of having to remember IP addresses.

Example:

```text
User
  ↓
example.com
  ↓
DNS / Route 53
  ↓
IP address / AWS resource
  ↓
Application
```

---

## Why Do We Need DNS?

Servers and other resources can be identified using IP addresses, but remembering long IP addresses is inconvenient.

Instead of accessing an application using something like:

```text
http://203.0.113.10
```

users can access it using:

```text
https://example.com
```

DNS resolves the domain name to the appropriate destination.

---

## DNS Records

DNS stores **records** that provide information about a domain.

A simplified example:

```text
example.com
     ↓
DNS Record
     ↓
IP address / target
```

The DNS record tells the DNS system where the request should be directed.

---

## Basic Request Flow

A simplified AWS application flow can look like:

```text
User
  ↓
example.com
  ↓
Route 53
  ↓
DNS resolution
  ↓
Load Balancer
  ↓
Application
```

Route 53 handles the DNS part of the process, while the Load Balancer and application handle the actual application traffic.

---

## Route 53 and DevOps

Route 53 is relevant to DevOps and Cloud Engineering because DNS is commonly used when exposing applications and services.

It can be used as part of architectures involving:

* Load Balancers
* EC2
* ECS
* CloudFront
* AWS-hosted applications
* Custom domains

Understanding DNS is important when troubleshooting application accessibility and traffic flow.

---

## Key Takeaways

* **Route 53 is AWS's DNS service.**
* DNS translates human-readable domain names into information that can be used to reach the appropriate destination.
* DNS uses records to store this information.
* DNS avoids the need for users to remember server IP addresses.
* Route 53 can direct users toward AWS resources such as Load Balancers.
* Route 53 is a networking and application-access component, not the application server itself.

### Remember

> **DNS answers: "Where should this domain name go?"**

```text
Domain
  ↓
Route 53 / DNS
  ↓
DNS Record
  ↓
Target
  ↓
Application
```

## Learning Status

**Day 6: Route 53 basic overview completed.**

Covered:

* DNS
* Route 53
* DNS records
* Domain name to destination resolution
* Basic DNS request flow
* Route 53's role in AWS application architecture
