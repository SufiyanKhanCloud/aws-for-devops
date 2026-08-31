# Amazon VPC

## What is VPC?

Amazon VPC (Virtual Private Cloud) is a logically isolated virtual network in AWS where AWS resources can be deployed and network communication can be controlled.

```text
AWS Region
    ↓
   VPC
    ↓
Subnets
    ↓
AWS Resources
```

---

## Core Components

* CIDR
* Subnets
* Route Tables
* Internet Gateway
* NAT Gateway
* Security Groups
* Network ACLs

---

## CIDR

A VPC requires an IP address range defined using CIDR notation.

Example:

```text
10.0.0.0/16
```

Subnets can then be created from this larger range.

---

## Subnets

Subnets divide a VPC into smaller network segments.

Each subnet belongs to a single Availability Zone.

Example:

```text
VPC
│
├── AZ-1
│   ├── Public Subnet
│   └── Private Subnet
│
└── AZ-2
    ├── Public Subnet
    └── Private Subnet
```

---

## Public vs Private Subnet

A subnet is considered public when its route table has a route through an Internet Gateway.

A private subnet does not have a direct route to an Internet Gateway.

```text
Public:

Subnet
 ↓
Route Table
 ↓
Internet Gateway
 ↓
Internet
```

Private resources can still obtain outbound internet access through a NAT Gateway.

```text
Private Subnet
 ↓
NAT Gateway
 ↓
Internet Gateway
 ↓
Internet
```

---

## Route Tables

Route tables determine where network traffic is sent.

Example:

```text
Destination       Target

10.0.0.0/16       local
0.0.0.0/0         Internet Gateway
```

Mental model:

> **Route Table = Where should this traffic go?**

---

## Internet Gateway

Provides a path between a VPC and the internet when appropriate routing and addressing are configured.

Attaching an Internet Gateway alone does not automatically make every subnet public.

---

## NAT Gateway

Allows resources in private subnets to initiate outbound internet connections without being directly reachable from the internet.

---

## Security Groups

Security Groups control network traffic at the resource level.

They are:

* Stateful
* Allow-rule based

Example:

```text
SSH
TCP 22
Source: trusted IP
```

---

## Network ACLs

Network ACLs operate at the subnet level.

They are:

* Stateless
* Support allow and deny rules

### Difference

```text
Security Group → Resource level → Stateful

NACL → Subnet level → Stateless
```

---

## High Availability

A VPC can span multiple Availability Zones.

A highly available application can distribute resources across multiple AZs.

```text
             Load Balancer
              /         \
             ↓           ↓
           AZ-1         AZ-2
            EC2          EC2
```

---

## Typical Architecture

```text
Internet
   ↓
Internet Gateway
   ↓
Public Subnet
   ↓
ALB
   ↓
Private Subnet
   ↓
EC2
   ↓
Private Database
```

The exact architecture should depend on the application requirements.

---

## DevOps Relevance

VPC knowledge is fundamental for:

* EC2
* ECS
* RDS
* Load Balancers
* Kubernetes
* Terraform
* Secure application architectures
* Troubleshooting connectivity

---

## Hands-on Status

**Theory completed.**

Practical VPC implementation and troubleshooting will be completed later.

Planned lab:

* Create VPC
* Create public and private subnets
* Configure route tables
* Configure Internet Gateway
* Configure NAT Gateway
* Configure Security Groups
* Test connectivity
* Break a configuration intentionally
* Troubleshoot the failure

---

## Key Takeaway

> **VPC provides the network, subnets provide segmentation, route tables determine traffic paths, gateways provide connectivity, and security controls determine what traffic is allowed.**
