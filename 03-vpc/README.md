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

A VPC gives us control over important networking components such as IP addressing, subnets, routing, and network traffic controls.

---

## Core Components

The main VPC components covered so far are:

* CIDR
* Subnets
* Route Tables
* Internet Gateway
* NAT Gateway
* Security Groups
* Network ACLs

---

# CIDR

A VPC requires an IP address range defined using CIDR notation.

Example:

```text
10.0.0.0/16
```

The VPC CIDR provides the overall IP address range from which smaller subnet ranges can be created.

Example:

```text
VPC: 10.0.0.0/16

        ↓

Public Subnet:  10.0.1.0/24
Private Subnet: 10.0.2.0/24
```

---

# Subnets

Subnets divide a VPC into smaller network segments.

Each subnet belongs to a **single Availability Zone**.

A VPC can contain subnets across multiple Availability Zones.

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

Using multiple Availability Zones is important when designing highly available architectures.

---

# Public vs Private Subnet

A subnet is considered **public** when its associated route table has a route to an Internet Gateway.

A private subnet does not have a direct route to an Internet Gateway.

### Public Subnet

```text
Subnet
   ↓
Route Table
   ↓
Internet Gateway
   ↓
Internet
```

### Private Subnet

Private resources can still initiate outbound internet connections through a NAT Gateway.

```text
Private Subnet
   ↓
NAT Gateway
   ↓
Internet Gateway
   ↓
Internet
```

A private subnet is not made public simply because it contains an EC2 instance with an IP address. Routing and addressing determine how connectivity works.

---

# Route Tables

Route tables determine where network traffic is sent.

Example:

```text
Destination       Target

10.0.0.0/16       local
0.0.0.0/0         Internet Gateway
```

The `local` route allows communication within the VPC CIDR.

The `0.0.0.0/0` route represents traffic destined for anywhere not covered by a more specific route.

### Mental Model

> **Route Table = Where should this traffic go?**

---

# Internet Gateway

An Internet Gateway (IGW) provides a path between a VPC and the internet when appropriate routing and addressing are configured.

An Internet Gateway alone does **not** automatically make every subnet public.

For internet connectivity, the relevant subnet must have appropriate routing, and the resource must have appropriate addressing and network permissions.

---

# NAT Gateway

A NAT Gateway allows resources in private subnets to initiate outbound connections to the internet without allowing unsolicited inbound internet connections directly to those private resources.

Typical architecture:

```text
Private Subnet
      ↓
 NAT Gateway
      ↓
Internet Gateway
      ↓
  Internet
```

NAT Gateway is commonly used when private workloads need outbound internet access for activities such as downloading packages or contacting external APIs.

---

# Security Groups

A Security Group acts as a virtual firewall associated with resources such as EC2 instances.

It controls network traffic at the **resource level**.

### Key Characteristics

* Stateful
* Supports allow rules
* Controls inbound and outbound traffic
* Associated with network interfaces/resources
* Rules determine whether traffic is permitted

Example:

```text
Browser
   ↓
TCP :8000
   ↓
Security Group
   ↓
EC2
```

If TCP port `8000` is not allowed by the Security Group, the request cannot reach the application even if the NACL permits it.

---

## Stateful Behavior

Security Groups are **stateful**.

When an inbound connection is allowed, the response traffic is automatically allowed back because the Security Group tracks the connection.

This means you do not normally need to create a separate outbound rule specifically to allow the response to an allowed inbound connection.

### Mental Model

> **Security Group = Resource-level stateful firewall**

---

# Network ACL

A Network ACL (NACL) controls traffic at the **subnet level**.

It acts as an additional layer of network traffic control for the subnet.

### Key Characteristics

* Stateless
* Supports both allow and deny rules
* Operates at the subnet level
* Inbound and outbound traffic are evaluated separately
* Rules are evaluated in rule-number order

Example:

```text
Internet
   ↓
NACL
   ↓
Subnet
   ↓
EC2
```

---

## Stateless Behavior

NACLs are **stateless**.

Inbound and outbound traffic are evaluated independently.

Therefore, if traffic is allowed in one direction, the return traffic must also be permitted by the appropriate NACL rules.

### Mental Model

> **NACL = Subnet-level stateless traffic filter**

---

# Security Group vs Network ACL

Security Groups and Network ACLs are different layers of network traffic control.

They should not be treated as interchangeable.

| Feature         | Security Group                                        | Network ACL                                      |
| --------------- | ----------------------------------------------------- | ------------------------------------------------ |
| Level           | Resource                                              | Subnet                                           |
| Stateful        | Yes                                                   | No                                               |
| Rules           | Allow                                                 | Allow + Deny                                     |
| Rule evaluation | Rules are evaluated together according to SG behavior | Lowest numbered matching rule is evaluated first |
| Common use      | Control access to individual workloads                | Additional subnet-level traffic control          |

### Mental Model

```text
NACL
 ↓
"Is this traffic permitted at the subnet level?"

Security Group
 ↓
"Is this traffic permitted to reach this resource?"
```

For a connection to succeed, relevant routing and network controls along the traffic path must allow it.

---

# NACL Rule Numbers

NACL rules are evaluated in **ascending numerical order**.

Example:

```text
Rule 100
   ↓
Rule 200
   ↓
Rule 300
```

The first matching rule determines the result.

Therefore, rule numbering matters when designing NACL rules.

For example, if a traffic pattern matches rule `100`, a later rule such as `200` will not be evaluated for that traffic.

---

# Security Group and NACL Traffic Flow

A useful troubleshooting mental model is:

```text
Internet
    │
    ▼
VPC / Subnet
    │
    ├── Network ACL
    │
    ▼
EC2 Network Interface
    │
    └── Security Group
            │
            ▼
       EC2 Instance
            │
            ▼
      Application :8000
```

The exact packet-processing path depends on the traffic direction and AWS networking implementation, but the important operational concept is:

> **Both subnet-level and resource-level network controls can affect whether connectivity succeeds.**

---

# High Availability

A VPC can span multiple Availability Zones.

A highly available application can distribute resources across multiple AZs so that failure of one Availability Zone does not necessarily make the entire application unavailable.

Example:

```text
             Load Balancer
              /         \
             ↓           ↓
           AZ-1         AZ-2
            EC2          EC2
```

High availability is achieved through the architecture deployed **within the VPC**, not simply by creating a VPC across multiple AZs.

---

# Typical AWS VPC Architecture

A common three-tier style architecture might look like:

```text
                    Internet
                       ↓
                Internet Gateway
                       ↓
                  Public Subnets
                       ↓
                    ALB
                   /   \
                  ↓     ↓
          Private App Subnets
              EC2 / ECS
                  ↓
          Private DB Subnets
                  ↓
             RDS Database
```

The exact architecture should always depend on the application requirements, security requirements, availability requirements, and cost constraints.

---

# Day 5 Hands-on Lab

## Objective

Understand how Security Groups and Network ACLs affect network connectivity by deliberately changing their rules and testing a simple application running on EC2.

---

## Lab Architecture

```text
                         Browser
                            │
                            │ TCP :8000
                            ▼
                         Internet
                            │
                            ▼
                          VPC
                            │
                            ▼
                      Public Subnet
                            │
                     ┌──────┴──────┐
                     │             │
                    NACL      Security Group
                     │             │
                     └──────┬──────┘
                            ↓
                       EC2 Instance
                            ↓
                Python HTTP Server :8000
```

---

## Step 1: Create VPC and EC2

Created a VPC using the basic/default networking configuration and launched an EC2 instance inside the VPC.

The purpose was to use the EC2 instance as a simple test workload for experimenting with network access controls.

---

## Step 2: Start a Python HTTP Server

Connected to the EC2 instance through SSH.

Updated the Ubuntu package information:

```bash
sudo apt update
```

Started a simple Python HTTP server:

```bash
python3 -m http.server 8000
```

The application was listening on:

```text
TCP 8000
```

---

## Step 3: Test with the Security Group

Initially, the Python server could not be accessed from the browser.

The important observation was:

```text
NACL → Allows the traffic
SG   → Does not allow inbound TCP 8000
        ↓
   Connection fails
```

This demonstrated that an application listening on a port does not automatically make that port reachable from the network.

The Security Group must permit the required traffic.

---

## Step 4: Allow TCP 8000 in the Security Group

Added an inbound Security Group rule allowing TCP port `8000`.

Example:

```text
Inbound Rule:

Protocol: TCP
Port: 8000
Source: Appropriate test source
```

After allowing the traffic:

```text
NACL → Allows
SG   → Allows TCP 8000
        ↓
Application reachable
```

The Python HTTP server became accessible from the browser.

This demonstrated the effect of a Security Group at the resource level.

---

## Step 5: Block Traffic Using the NACL

After allowing TCP `8000` at the Security Group level, traffic was blocked using the NACL.

Result:

```text
NACL → Blocks
SG   → Allows TCP 8000
        ↓
   Connection fails
```

This demonstrated that allowing traffic in the Security Group is not sufficient if another network control on the traffic path blocks the connection.

---

## Step 6: NACL Rule Numbers

The NACL rules were examined using their rule numbers.

Example:

```text
Rule 100
Rule 200
Rule 300
```

The lower-numbered rule is evaluated first.

The first matching rule determines whether the traffic is allowed or denied.

This demonstrated why NACL rule ordering is important.

---

# What the Lab Demonstrated

The most important lesson was not simply the difference between a Security Group and a NACL.

The important lesson was understanding that **multiple layers can affect the same network connection**.

A simplified troubleshooting model is:

```text
Traffic
   ↓
Routing must work
   ↓
NACL must permit traffic
   ↓
Security Group must permit traffic
   ↓
Application must be listening
   ↓
Connection succeeds
```

If an important layer blocks the traffic:

```text
NACL blocks       ❌
       OR
SG blocks         ❌
       OR
Routing fails     ❌
       OR
Application down  ❌
       ↓
Connection fails
```

---

# Troubleshooting EC2 Connectivity

When an application running on EC2 cannot be reached, do not immediately assume the Security Group is the problem.

Follow the traffic path systematically.

```text
1. Is the EC2 instance running?
        ↓
2. Is the application running?
        ↓
3. Is the application listening on the expected port?
        ↓
4. Is the correct IP address / hostname being used?
        ↓
5. Is the subnet routing correct?
        ↓
6. Does the NACL permit the traffic?
        ↓
7. Does the Security Group permit the traffic?
        ↓
8. Is there any other network or host-level firewall blocking it?
```

This layered approach is useful for real-world AWS networking troubleshooting.

---

# Day 4 and Day 5 Learning Status

### Day 4

**VPC theory completed.**

Covered:

* VPC
* CIDR
* Subnets
* Public vs private subnets
* Route tables
* Internet Gateway
* NAT Gateway
* Security Groups
* Network ACLs
* Availability Zones
* Basic VPC architecture

### Day 5

**Security Group + NACL theory and hands-on completed.**

Hands-on included:

* Creating a VPC
* Launching EC2 inside the VPC
* Running a Python HTTP server
* Testing access to TCP port `8000`
* Testing Security Group behavior
* Testing NACL behavior
* Testing NACL rule-number ordering
* Troubleshooting connectivity

A deeper VPC lab covering subnet design, route tables, Internet Gateway, NAT Gateway, and multi-AZ architecture will be completed later.

---

# DevOps Relevance

VPC knowledge is fundamental for:

* EC2
* ECS
* RDS
* Load Balancers
* Kubernetes
* Terraform
* CI/CD infrastructure
* Secure application architectures
* Network troubleshooting
* High availability
* Private workloads
* Production infrastructure

As a DevOps/Cloud Engineer, VPC knowledge is especially important because many deployment problems eventually become networking problems.

Examples include:

```text
EC2 cannot reach internet
        ↓
Routing / NAT / IGW investigation

Application cannot be reached
        ↓
Route / NACL / SG / OS firewall / application investigation

ECS task cannot reach database
        ↓
Subnet / route / SG / DNS / database investigation

CI/CD runner cannot access AWS resource
        ↓
IAM + network + security investigation
```

---

# What I Should Understand Deeply

For VPC fundamentals, the following concepts should be understood rather than memorized:

* CIDR and IP addressing
* Public vs private subnets
* Route tables
* Internet Gateway
* NAT Gateway
* Security Groups
* Network ACLs
* Stateful vs stateless filtering
* NACL rule evaluation
* Availability Zones
* Network traffic flow
* Layered troubleshooting

The goal is to be able to look at a connectivity problem and reason:

```text
Requirement
    ↓
Traffic source
    ↓
Traffic destination
    ↓
Route
    ↓
Network controls
    ↓
Application
    ↓
Result
```

---

# Common Traps

### Trap 1: "Internet Gateway makes a subnet public."

Not by itself.

The subnet also needs appropriate routing, and the workload needs appropriate addressing and network permissions.

---

### Trap 2: "Security Group and NACL are the same thing."

They operate at different levels and have different behavior.

```text
Security Group → Resource level → Stateful

NACL → Subnet level → Stateless
```

---

### Trap 3: "If the Security Group allows it, the connection must work."

Not necessarily.

Routing, NACLs, host firewalls, the application itself, and other dependencies can still cause failure.

---

### Trap 4: "A private subnet cannot access the internet."

A private subnet can have outbound internet connectivity through a NAT Gateway without providing direct inbound internet connectivity to the private resources.

---

### Trap 5: "All NACL rules are evaluated."

NACL rules are evaluated in ascending rule-number order, and the first matching rule determines the result.

---

# Key Takeaways

* **VPC** provides an isolated virtual network in AWS.
* **CIDR** defines the VPC's IP address range.
* **Subnets** divide the VPC into smaller network segments.
* A subnet is associated with one **Availability Zone**.
* **Route tables** determine where traffic goes.
* **Internet Gateway** provides internet connectivity for appropriately configured public resources.
* **NAT Gateway** provides outbound internet access for private resources.
* **Security Groups** operate at the resource level and are stateful.
* **NACLs** operate at the subnet level and are stateless.
* Security Groups support allow rules, while NACLs support allow and deny rules.
* NACL rules are evaluated from the lowest rule number upward.
* Multiple network controls can affect the same connection.
* Troubleshooting should follow the complete traffic path rather than focusing on only one component.
* High availability requires distributing workloads across multiple Availability Zones when appropriate.

## Remember

> **VPC provides the network, subnets provide segmentation, route tables determine traffic paths, gateways provide connectivity, and security controls determine what traffic is allowed.**

> **Security Group = resource-level, stateful firewall.**

> **NACL = subnet-level, stateless traffic filter.**
