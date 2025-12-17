CHAPTER 6 
CHAPTER 6 
Amazon Networking 
A. Amazon VPC 
B. Route53 
C. CloudFront 
COCoderCo. 
 

 

  CIDR (Classless Inter-Domain Routing) — Key Notes 

What is CIDR? 

A method for allocating and defining IP address ranges. 

Written as: 
IP address + slash + number (example: 192.168.0.0/24) 

 
 

How CIDR Works 

CIDR has two parts: 

1. Base IP 

The starting IP of the range. 

Example: 10.0.0.0, 192.168.0.0 

2. Subnet Mask (the / number) 

Defines how many bits are fixed and how many bits can change. 

Controls the size of the IP range. 

 
 

CIDR Examples 

/32 → 1 single IP 

Example: 192.168.1.10/32 

/0 → all IP addresses 

Example: 0.0.0.0/0 = everything (used for “allow all”) 

/26 → 64 IPs 

Common ones: 

/8 → very large range 

/16 → medium range 

/24 → 256 IPs 

/32 → single IP 

 
 

Key Rule 

Smaller slash number → BIGGER IP range 

Larger slash number → SMALLER IP range 

 

////////////////////////////////////////////////////// 

 

CIDR & Subnet Mask Basics — Key Notes 

How Subnet Masks Control IP Range 

The /number determines how many IPs are in the range. 

Larger number → smaller range 

Smaller number → larger range 

 
 

IP Count Examples 

/32 → 1 IP 

/31 → 2 IPs 

/30 → 4 IPs 

/29 → 8 IPs 

/24 → 256 IPs 

/16 → 65,536 IPs 

(Every time the mask decreases by 1, the number of IPs doubles.) 

 
 

Common CIDRs in AWS VPCs 

/24 → 256 IPs (common for subnets) 

/16 → 65,536 IPs (common for whole VPCs) 

 
 

Using Octets to Understand Ranges 

Think of the IP as 4 octets: 

CIDR 

What Can Change 

Meaning 

/0 

All 4 octets 

Entire IPv4 range 

/8 

Last 3 octets 

Large network 

/16 

Last 2 octets 

Medium-large network 

/24 

Last 1 octet 

Small subnet 

/32 

No octets 

Single IP 

 
 

Why This Matters 

Essential when designing VPCs and subnets. 

Helps allocate: 

Public subnets 

Private subnets 

Routing ranges 

 

Subnet Mask Basics 

The subnet mask allows part of the underlying IP to generate additional next values from the base IP. 

 
 

CIDR Examples (Using Base IP: 192.168.0.0) 

/32 → allows 1 IP (2⁰) 
→ Range: 192.168.0.0 

/31 → allows 2 IPs (2¹) 
→ Range: 192.168.0.0 → 192.168.0.1 

/30 → allows 4 IPs (2²) 
→ Range: 192.168.0.0 → 192.168.0.3 

/29 → allows 8 IPs (2³) 
→ Range: 192.168.0.0 → 192.168.0.7 

/28 → allows 16 IPs (2⁴) 
→ Range: 192.168.0.0 → 192.168.0.15 

/27 → allows 32 IPs (2⁵) 
→ Range: 192.168.0.0 → 192.168.0.31 

/26 → allows 64 IPs (2⁶) 
→ Range: 192.168.0.0 → 192.168.0.63 

/25 → allows 128 IPs (2⁷) 
→ Range: 192.168.0.0 → 192.168.0.127 

/24 → allows 256 IPs (2⁸) 
→ Range: 192.168.0.0 → 192.168.0.255 

 
 

/16 → allows 65,536 IPs (2¹⁶) 
→ Range: 192.168.0.0 → 192.168.255.255 

/0 → allows ALL IPs 
→ Range: 0.0.0.0 → 255.255.255.255 

 
 

Octet Rules 

CIDR 

Octet Behaviour 

/32 

No octet can change 

/24 

Last octet can change 

/16 

Last 2 octets can change 

/8 

Last 3 octets can change 

/0 

All octets can change 

 

 

///////////////////////////////////////// 

 

  Subnets in a VPC — Key Notes 

What is a Subnet? 

A subnet = a smaller network inside your VPC. 

Used to group resources and spread them across multiple Availability Zones for high availability. 

Subnets can be: 

Public → has internet access 

Private → no direct internet access 

 
 

AWS Reserves 5 IP Addresses in Every Subnet 

AWS automatically takes 5 IPs from each subnet, so these cannot be used by EC2 or other resources. 

Example: Subnet 10.0.0.0/24 

IP Address 

Reserved For 

.0 

Network address 

.1 

VPC router 

.2 

Amazon DNS 

.3 

AWS future use 

.255 

Broadcast address (not supported by AWS → reserved) 

So with /24 (256 IPs), only 251 are usable. 

 
 

Choosing the Right Subnet Size 

Always remember: 

Usable IPs = Total IPs − 5 

Example: 

/28 = 16 IPs → 11 usable 

/27 = 32 IPs → 27 usable 

/26 = 64 IPs → 59 usable 

 
 

Important Planning Tip 

If you need 29 IPs, /27 will NOT work: 

/27 → 32 total → 27 usable → ❌ not enough 

Instead choose: 

/26 → 64 total → 59 usable → ✅ enough for 29 instances 

 
 

Quick Memory Trick 

AWS always keeps first 4 + last IP in every subnet. 

So when planning subnets, always subtract 5 from the CIDR size. 

 

//////////////////////// 

 

Bastion Hosts — Key Notes 

What is a Bastion Host? 

A secure bridge used to access private EC2 instances inside a private subnet. 

Lives in a public subnet, so you can access it from the internet. 

From the bastion, you SSH into private instances that cannot be reached directly from outside. 

 
 

Why Do We Need a Bastion Host? 

Private instances (databases, backend servers) cannot be exposed to the internet. 

But admins still need access for: 

Updates 

Maintenance 

Troubleshooting 

Bastion provides controlled, secure access without exposing private instances. 

 
 

How It Works 

Create bastion host in public subnet. 

Connect to it via SSH from your allowed IPs. 

From bastion → SSH into private EC2 instances using their private IPs. 

 
 

Security Group Requirements 

Bastion Host SG 

Allows inbound SSH (port 22) 

Only from trusted IP ranges (e.g., office IP) 

Private Instance SG 

Allows inbound SSH 

Only from bastion host’s private IP or bastion SG 

This prevents ANY external SSH access to private servers. 

 
 

Benefits 

Private resources stay unreachable from the internet 

Strong separation of public vs private access 

Great for sensitive systems (databases, internal apps) 

Reduces attack surface 

 

/////////////// 

 

NAT Gateway — Key Notes 

What is a NAT Gateway? 

NAT = Network Address Translation 

Allows private subnet instances to access the internet (updates, APIs, downloads). 

Blocks all inbound connections from the internet → keeps private instances secure. 

Fully managed by AWS (no maintenance, patching, or scaling needed). 

 
 

Why Do We Use It? 

Private instances must stay isolated from the internet for security. 

But they still need outbound access for: 

OS updates 

Package installs 

External API calls 

NAT Gateway provides outbound internet only, with no inbound exposure. 

 
 

Important Characteristics 

✅ Managed by AWS 

Handles high availability, fault tolerance, and scaling. 

Zero maintenance compared to NAT Instances. 

✅ Billing 

Charged per hour + per GB of traffic. 

Heavy traffic → higher cost. 

✅ AZ-Specific 

NAT Gateways are tied to one Availability Zone. 

For high availability → deploy one NAT Gateway per AZ. 

✅ Elastic IP Required 

Each NAT Gateway uses a public Elastic IP to access the internet. 

✅ Requires an Internet Gateway 

NAT Gateway → Internet Gateway → Internet 

Without an Internet Gateway, NAT Gateway cannot reach the internet. 

✅ Subnet Requirements 

Must be placed in a public subnet. 

Only private subnets can use it via route tables. 

Instances in the same subnet as the NAT Gateway cannot use it. 

✅ Bandwidth 

Starts at 5 Gbps, auto-scales up to 100 Gbps. 

✅ No Security Groups 

NAT Gateway does not use or require security groups. 

Traffic rules handled through route tables. 

 
 

Typical Architecture 

NAT Gateway lives in a public subnet. 

Private subnet route table sends 0.0.0.0/0 → NAT Gateway. 

NAT Gateway sends traffic → Internet Gateway → Internet. 

Private instances can access internet but are not reachable from outside. 

 
 

Summary 

NAT Gateways allow outbound-only internet access for private subnets while preserving strict security. They are managed, scalable, reliable, and ideal for secure environments where EC2 instances must remain private. 

 

//////////// 

 

Feature 

NAT Gateway 

NAT Instance 

Availability 

Highly available within an AZ (create multiple for multi-AZ) 

Requires custom scripts for failover 

Bandwidth 

Up to 100 Gbps 

Depends on EC2 instance type 

Maintenance 

Managed entirely by AWS 

Managed by you (patches, software, OS updates) 

Cost 

Charged per hour + data processed 

Charged per hour based on EC2 type + network costs 

Public IPv4 

✔️ 

✔️ 

Private IPv4 

✔️ 

✔️ 

Security Groups 

❌ Not supported 

✔️ Supported 

Use as Bastion Host? 

❌ Not possible 

✔️ Can be used 

 

///////////////////////////////////////////////////// 

 

  

 Network Access Control List (NACL) 

A Network ACL is like a large filter at the subnet level. It decides whether traffic entering or leaving a subnet should be allowed or denied, based on rules you define. 
Every subnet automatically receives a default NACL, but you can create custom ones. 

 
 

Key Characteristics of NACLs 

1️⃣ One NACL per Subnet 

Each subnet can only be associated with one NACL. 

Whatever rules are in that NACL apply to all resources inside that subnet. 

2️⃣ Stateless 

Unlike security groups, NACLs do not keep track of traffic sessions. 

This means: 

If you allow inbound traffic, you must explicitly allow outbound traffic for the return path. 

Both directions must have rules. 

 
 

How NACL Rules Work 

🔢 Numbered Rules 

Rules are numbered from 1 to 32,766. 

Lower numbers = higher precedence. 

🏆 First Match Wins 

As soon as traffic matches a rule, that rule is applied. 

No further rules are checked. 

Example: 

Rule 100: ALLOW 10.0.0.10/32 

Rule 200: DENY 10.0.0.10/32 
➡️ Rule 100 applies because it has the lower number. 

⭐ Last Rule = Catch-All ( Rule)* 

Always evaluated last. 

Usually DENY ALL by default. 

This is why a new custom NACL blocks everything until you add specific rules. 

 
 

Best Practices 

✔️ Use Incremental Rule Numbers 

Use spacing like 100, 200, 300… 

Allows room to insert new rules later (e.g., add rule 150 between 100 and 200). 

✔️ Use NACLs for Subnet-Level Blocking 

Ideal for blocking: 

Malicious IP addresses 

Entire IP ranges 

Traffic types to whole subnets 

Helps add a second layer of defense before traffic reaches EC2 instances. 

 
 

Summary 

Network ACLs provide subnet-wide traffic filtering. 
They are stateless, rule-based, and processed by the first-match-wins method. 
They are perfect when you need to block or allow traffic at scale, affecting all resources inside a subnet. 

 

//////////////////////////////////////// 

 

Security Groups (SG) vs Network ACLs (NACL) 

Understanding how traffic flows through SGs and NACLs is one of the most important concepts in AWS networking. 
Most connectivity issues in AWS come from a misconfigured SG or NACL — so this needs to be crystal clear. 

 
 

1. Scope of Control 

Security Groups (SG) 

Work at the instance level (EC2, ENI, Lambda ENIs, etc.) 

Control traffic directly to/from each instance 

Think of SGs as a firewall on the server itself 

NACLs (Network Access Control Lists) 

Work at the subnet level 

Control traffic entering or leaving the entire subnet 

Affect all resources inside that subnet 

Think of NACLs as a door at the subnet boundary 

 
 

2. Stateful vs Stateless 

Security Groups – STATEFUL 

If you allow inbound, the outbound response is automatically allowed 

You do not need matching outbound rules 
✔ No need to create reverse rules 
✔ Much easier to manage 

NACLs – STATELESS 

If you allow inbound, you MUST also allow outbound 

If you allow outbound, you MUST also allow inbound 
❗ Both directions must be configured explicitly 

This is the main reason people misconfigure NACLs. 

 
 

Traffic Flow Explained (with the diagram logic) 

 
 

🔵 Incoming Request (Traffic from the internet → EC2) 

Step 1: NACL Inbound Rules 

Traffic hits the subnet boundary first 

NACL checks inbound rules (stateless) 

If allowed → traffic moves inward 

If denied → traffic blocked immediately 

Step 2: Security Group Inbound Rules 

After passing NACL, traffic goes to the SG on the instance 

If SG allows the request → EC2 receives it 

If SG denies → request blocked at instance level 

Step 3: Outbound Reply 

SG is stateful → reply is automatically allowed 
BUT… 

Step 4: NACL Outbound Rules 

Because NACLs are stateless, the outbound traffic MUST be explicitly allowed 
If the outbound rule doesn't allow it → the response is dropped 

 
 

🔴 Outgoing Request (EC2 → Internet) 

Step 1: Security Group Outbound Rules 

The instance must have SG outbound rules permitting the traffic 

If allowed → traffic leaves the instance 

Step 2: NACL Outbound Rules 

Must explicitly allow outbound traffic 

If allowed → leaves subnet 

If blocked → never reaches the internet 

Step 3: Return Traffic (Inbound Response) 

Return packet comes back into the subnet 

NACL inbound rules must explicitly allow it 

If allowed → forwarded to SG 

If blocked → response is dropped before SG sees it 

Step 4: SG Inbound 

SG automatically accepts return traffic because it's stateful 

 
 

The Big Summary (The Part You MUST Remember) 

✔ Security Groups are STATEFUL 

Allow inbound → outbound automatically allowed 

No need to write reverse rules 

Instance level security 

✔ NACLs are STATELESS 

Must allow inbound AND outbound 

First matching rule wins 

Subnet-wide security 

✔ SG problems = instance access issues 

✔ NACL problems = subnet-wide access issues 

If something cannot connect to the internet or cannot be reached, ALWAYS check SG + NACL. 

They work together to provide layered security. 

 

 

 ////////////////////////////////////////////// 

 

 

 VPC Peering — Explained Simply 

What is VPC Peering? 

VPC peering is a way to privately connect two VPCs using AWS’s internal network. 
Once connected, the two VPCs can communicate as if they were on the same network, without using the internet. 

 
 

Key Features of VPC Peering 

1. Private Connection 

All traffic stays inside AWS’s private backbone network 

No internet exposure 

More secure and faster 

2. No Overlapping CIDRs 

The VPCs must not have overlapping IP ranges 

If they overlap, AWS will not allow the peering connection 

3. Peering is Non-Transitive 

This is the BIG rule people forget: 

If: 

VPC A peers with VPC B 

VPC B peers with VPC C 

➡️ A cannot talk to C 
➡️ You must create a direct peering connection between A and C 

Peering works only between the two VPCs directly connected. 

4. Route Tables Must Be Updated 

Creating the peering connection alone is not enough. 
You must update route tables in both VPCs so that each subnet knows how to reach the other VPC. 

 
 

Why Use VPC Peering? — Real Scenarios 

1. Departments Sharing Resources 

Example: 

Sales team VPC 

Marketing team VPC 

If Sales needs to read data from a Marketing database → VPC peering allows secure internal access without touching the internet. 

2. Cross-Account or Environment Connectivity 

Common setup: 

Dev in one VPC 

Production in another 

Both need to share certain resources (e.g., logging, internal APIs) 

Peering gives them a secure internal connection. 

 
 

Summary (Fast Revision) 

VPC Peering = Private connection between two VPCs 

No overlapping CIDRs allowed 

Non-transitive → all VPCs must be directly peered 

Route tables MUST be updated 

Used for cross-VPC communication without internet 

 

///////////////////////////////////// 

 

 

AWS VPC Endpoints — Explained Simply 

AWS provides two main types of VPC endpoints to let your resources access AWS services privately (without using the public internet): 

Interface Endpoints (powered by PrivateLink) 

Gateway Endpoints 

Let’s break them down. 

 
 

1️⃣ Interface Endpoints (PrivateLink) 

What they are 

Create an Elastic Network Interface (ENI) inside your subnet 

That ENI gets a private IP, which becomes the entry point for your private resources 

Allows private access to many AWS services (SNS, SQS, Secrets Manager, ECR API, CloudWatch, etc.) 

How they work 

Your EC2 in a private subnet talks to the ENI 

The ENI forwards traffic privately to the AWS service 

No internet involved 

Traffic stays on AWS’s private backbone 

Security 

You must attach a Security Group to the ENI 

SG controls what can access the endpoint 

Costs 

You pay for: 

Hourly charges 

Data processing (per GB) 

👉 Can get expensive if large amounts of data flow through. 

Best for 

Services other than S3/DynamoDB 

Private access to SNS, SQS, Secrets Manager, KMS, API Gateway private API, etc. 

When you need extreme security and no internet routing 

 
 

2️⃣ Gateway Endpoints 

What they are 

A “gateway” object that you reference in your route tables 

Does NOT create an ENI 

Does NOT need security groups 

Supported Services 

Only supports: 

Amazon S3 

DynamoDB 

How they work 

You add routes like: 

Destination: S3 prefix list → Target: Gateway Endpoint 

EC2 traffic stays inside AWS (private) and does not use NAT Gateway or Internet Gateway 

Costs 

Completely FREE 

No hourly charges 

No per-GB charges 

Best for 

Accessing S3 and DynamoDB from private subnets securely and cheaply 

Reducing NAT Gateway costs 

 
 

🆚 Summary Table — Interface vs Gateway Endpoints 

Feature 

Interface Endpoint (PrivateLink) 

Gateway Endpoint 

Uses ENI 

✅ Yes 

❌ No 

Security Groups 

✅ Required 

❌ Not used 

Cost 

💰 Hourly + per GB 

FREE 

Supported Services 

Most AWS services 

Only S3 + DynamoDB 

Route Table Needed 

Optional (DNS-based) 

✅ Yes 

Best For 

Secure access to SNS, SQS, Secrets Manager, ECR 

Cheap private access to S3/DynamoDB 

 
 

🔑 Fast Revision Summary 

Interface Endpoint = ENI + SG + paid + supports most services 

Gateway Endpoint = free + only S3/DynamoDB + route table rule 

Both keep traffic private inside AWS 

Both eliminate the need for Internet Gateway OR NAT Gateway for those services 

 

/////////////////////////////////////// 

 

 

IPv6 in a VPC — Dual Stack Mode & How It Works 

1. IPv4 Cannot Be Disabled 

Even if you enable IPv6, IPv4 always remains active in AWS VPCs. 

You cannot create a VPC or subnet that is IPv6-only. 

 
 

2. Dual Stack Mode 

When IPv6 is enabled: 

Your VPC and subnets run in dual stack mode 

This means your resources use both IPv4 and IPv6 simultaneously 

Perfect for environments transitioning gradually to IPv6 

 
 

3. EC2 Instance Addressing 

When an EC2 instance has IPv6 enabled, it receives: 

✔ A private IPv4 address 

Used for internal communication inside the VPC 

Never leaves AWS’s private network 

✔ A public IPv6 address 

Globally routable on the public internet 

No NAT needed for IPv6 traffic 

How it reaches the internet 

The Internet Gateway supports both IPv4 and IPv6 

So the instance can communicate over either protocol 

 
 

4. Why Dual Stack Is Important 

Ensures full compatibility with IPv4-based systems (still the majority today) 

Enables future migration to IPv6 without breaking existing services 

Allows communication with both IPv4 and IPv6 clients 

Avoids address exhaustion issues common with IPv4 

 
 

Summary 

IPv4 stays on — it cannot be disabled 

IPv6 runs alongside IPv4 in dual stack mode 

EC2 instances get private IPv4 + public IPv6 

Internet Gateway supports both 

Dual stack mode ensures smooth IPv6 adoption while remaining backward compatible 

 

//////////////////////////////// 

 

IPv6 Troubleshooting 

IPv4 cannot be disabled for your VPC and subnets 

So, if you cannot launch an EC2 instance in your subnet: 

It’s not because it cannot acquire an IPv6 (the space is very large) 

It’s because there are no available IPv4 addresses in your subnet 

Solution: Create a new IPv4 CIDR in your subnet. 

 
 

VPC Example 

IPv4: 192.168.0.0/24 

IPv4: 10.0.0.0/24 

IPv6: 2001:db8:1234:5678::/56 

EC2 instances examples: 

192.168.0.10 

192.168.0.15 

10.0.0.35 

Users → Create → VPC (with IPv4 + IPv6 ranges) 

 

///////////////////////////// 

 

Egress-Only Internet Gateway (EOIGW) — IPv6 Outbound Security 

An Egress-Only Internet Gateway is an AWS component used exclusively for IPv6 traffic. It provides outbound-only internet access for instances using IPv6, while blocking all inbound connections. 
Think of it as the IPv6 version of a NAT Gateway, but only one-way. 

 
 

✅ Key Concepts 

1. Only for IPv6 

EOIGW is not used for IPv4. 

IPv4 uses NAT Gateway for outbound + blocked inbound. 

IPv6 does not need NAT because IPv6 addresses are globally routable—so EOIGW provides the necessary protection. 

 
 

2. Outbound Communication Only 

Allows instances in private subnets to: 

Access the internet (updates, APIs, downloads) 

Make external IPv6 requests 

Blocks all inbound IPv6 traffic. 

The internet cannot initiate connections into your VPC. 

This behavior mirrors a NAT gateway but applies only to IPv6. 

 
 

3. Route Table Update Required 

To use EOIGW, you must add a route such as: 

Destination: ::/0 
Target: Egress-Only Internet Gateway 
 

If you forget this, your IPv6-enabled instances cannot reach the internet. 

 
 

🔐 Why Use EOIGW? 

Because all IPv6 addresses are publicly routable, instances with IPv6 could otherwise be exposed to inbound traffic. 
EOIGW prevents that, giving you: 

Secure outbound-only IPv6 access 

No risk of unsolicited inbound public IPv6 connections 

It’s ideal for private subnets where you still need outgoing internet access. 

 
 

🧠 Summary 

Feature 

Egress-Only Internet Gateway 

Protocol 

IPv6 Only 

Purpose 

Outbound-only internet access 

Blocks inbound? 

Yes, all inbound IPv6 

Similar to 

NAT Gateway (but IPv6 + outbound only) 

Requires route updates? 

Yes (::/0 → EOIGW) 

 

////////////////////// 

 

 

 

 

/////////////////////////////// 

 

 

 

⭐ Dual-Stack VPC Architecture (IPv4 + IPv6) — How It Works 

In a dual-stack VPC, AWS assigns both IPv4 and IPv6 CIDR blocks to the VPC and to each subnet. 
This allows instances to communicate using either protocol, depending on what the destination supports. 

 
 

🧱 1. VPC and Subnet Configuration 

VPC: 

Has both an IPv4 CIDR (e.g., 10.0.0.0/16) 

And an IPv6 CIDR (e.g., 2001:db8:1234:5678::/56) 

Subnets: 

Each subnet receives: 

IPv4 CIDR block (e.g., 10.0.1.0/24) 

IPv6 CIDR block (e.g., 2001:db8:1234:5678:1::/64) 

This enables dual-stack operation inside each subnet. 

 
 

🌐 2. Public Subnet Behavior 

Instances in the public subnet get: 

A private IPv4 

A public IPv4 (Elastic IP) → internet access 

A global IPv6 address → also internet-routable 

Routing: 

Protocol 

Route 

Explanation 

IPv4 

IGW 

Public instance has an Elastic IP via IGW 

IPv6 

IGW 

IPv6 is always public & internet-routable 

Public instances can reach the internet over both IPv4 and IPv6 directly via the Internet Gateway. 

 
 

🔐 3. Private Subnet Behavior 

Instances in private subnets have: 

Private IPv4 

Private IPv6 (still globally unique, but not routable without a gateway) 

Routing: 

Protocol 

Gateway Used 

Why 

IPv4 

NAT Gateway 

To hide private IPv4; allow outbound-only 

IPv6 

Egress-Only Internet Gateway 

IPv6 cannot use NAT; EOIGW provides outbound-only access 

Thus, private instances cannot be reached from the internet, but can initiate outbound requests using either protocol. 

 
 

🚪 4. Gateways in Dual-Stack Architecture 

Internet Gateway (IGW) 

Handles: 

All IPv4 public traffic 

All IPv6 public traffic 

Used by: 

Public subnet instances 

Routing of IPv6 traffic ALWAYS goes through IGW (if it's public) 

 
 

NAT Gateway (IPv4 only) 

Handles: 

Outbound IPv4 from private subnets 

It: 

Converts private IPv4 → NAT’s Elastic IP 

Blocks inbound IPv4 from the internet 

 
 

Egress-Only Internet Gateway (IPv6 only) 

Handles: 

Outbound IPv6 from private subnets 

It: 

Allows outbound IPv6 

Blocks all inbound IPv6 

Think of EOIGW as: 
👉 “NAT Gateway for IPv6, but outbound only — no translation.” 

 
 

🗺️ 5. Route Tables Summary 

Public Subnet Route Table 

Destination 

Target 

0.0.0.0/0 

Internet Gateway 

::/0 

Internet Gateway 

 
 

Private Subnet Route Table 

Destination 

Target 

0.0.0.0/0 

NAT Gateway 

::/0 

Egress-Only Internet Gateway 

 
 

🧠 Summary — Dual-Stack VPC Logic 

Public Instances 

IPv4 → IGW 

IPv6 → IGW 

Fully internet-accessible (based on security group rules) 

Private Instances 

IPv4 → NAT Gateway 

IPv6 → Egress-Only IGW 

Outbound-only access for both protocols 

Not reachable from the internet 

Architectural Separation 

IPv4 depends on NAT for private → public 

IPv6 never uses NAT; it uses EOIGW for outbound-only protection 

 

/////////////////////////// 

 

⭐ VPC Networking – Ultimate Recap 

This recap ties together every core networking concept you covered in this section. Use it as your mental map for understanding how AWS networking works. 

 
 

🔹 CIDRs – Classless Inter-Domain Routing 

Defines the IP range for your VPC (IPv4 and IPv6). 

Sets boundaries for how many IPs your resources can use. 

Example: 10.0.0.0/16 or IPv6 2001:db8:1234::/56. 

 
 

🔹 VPC – Virtual Private Cloud 

Your own private network inside AWS. 

You allocate CIDRs and control routing, security, and connectivity. 

Think of it as your private data center in the cloud. 

 
 

🔹 Subnets 

Subdivide your VPC into smaller networks. 

Each subnet lives entirely inside one Availability Zone. 

Subnets can be public or private depending on route tables. 

 
 

🔹 Internet Gateway (IGW) 

Gives your VPC internet access for both IPv4 and IPv6. 

Attached at the VPC level. 

Required for public subnets. 

 
 

🔹 Route Tables 

Determine where traffic flows. 

You add routes to: 

Internet Gateway 

NAT Gateway 

VPC Peering 

VPC Endpoints 

Transit Gateway 

Subnets are associated with route tables, not the VPC directly. 

 
 

🔹 Bastion Host 

A secure bridge / jump box. 

Public EC2 instance used to SSH into private EC2 instances. 

Allows management access without exposing private resources. 

 
 

🔹 NAT Instance (Legacy) 

Older method to give private subnet instances IPv4 internet access. 

Requires manual setup: 

Disable Source/Dest Check 

Patch and maintain OS 

Scaling is manual 

 
 

🔹 NAT Gateway (Modern) 

AWS-managed, scalable, highly available. 

Allows private IPv4 outbound connections. 

No inbound connections allowed. 

Must exist in a public subnet with an Elastic IP. 

 
 

🔹 NACLs – Network Access Control Lists 

Stateless subnet-level firewalls. 

Must define rules for both inbound and outbound. 

First match wins. 

Good for blocking IPs at subnet level. 

 
 

🔹 Security Groups (SGs) 

Stateful instance-level firewalls. 

If inbound is allowed, return traffic is automatically allowed. 

Most common form of EC2 protection. 

 
 

🔹 VPC Peering 

Private connection between two VPCs using AWS backbone. 

Non-transitive: 

A ↔ B, B ↔ C does not allow A ↔ C 

No overlapping CIDRs allowed. 

Route tables must be updated. 

 
 

🔹 VPC Endpoints 

Provide private access to AWS services without internet: 

Gateway Endpoints 

For S3 and DynamoDB only. 

Free. 

Use route tables. 

Interface Endpoints (PrivateLink) 

Use ENIs with private IPs. 

Support many AWS services. 

Paid per hour + data processed. 

Use security groups. 

 
 

🔹 VPC Endpoint Services / PrivateLink 

Allows customers to connect to your service privately from their VPC. 

Requires NLB or ENI. 

No need for: 

VPC peering 

NAT 

Internet Gateway 

Ideal for SaaS service providers. 

 
 

🔹 Egress-Only Internet Gateway 

For IPv6 outbound only. 

IPv6 cannot use NAT, so this is the IPv6 equivalent of outbound-only access. 

Blocks inbound IPv6 traffic completely. 

 
 

🔹 Transit Gateway 

A hub-and-spoke model for routing. 

Provides transitive connectivity across: 

VPCs 

VPNs 

Direct Connect 

Solves the limitation of non-transitive VPC peering. 

Ideal for large multi-VPC architectures. 

 
 

⭐ Full Summary in One Sentence 

AWS VPC networking is built on CIDRs, subnets, routing, and layered security (NACLs + SGs), with internet access controlled by gateways (IGW, NAT, EOIGW), private connectivity enabled by endpoints and PrivateLink, and scalable multi-VPC routing powered by Transit Gateway. 

 
