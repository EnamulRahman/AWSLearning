Amazon CloudFront – Overview 

CloudFront is AWS’s Content Delivery Network (CDN) 

Purpose: Improve performance and security by delivering content closer to users 

 
 

How CloudFront Works 

Content is cached at Edge Locations (POPs – Points of Presence) 

Users are served content from the nearest edge location 

Reduces: 

Latency 

Load on the origin server 

Origins can be: 

S3 buckets 

EC2 / ALB 

Other HTTP servers 

 
 

Benefits 

Faster load times 

Better user experience 

Lower origin server load 

Global content delivery 

 
 

Security Features 

Integrated with: 

AWS Shield (DDoS protection) 

AWS WAF (Web Application Firewall) 

Protects applications from: 

DDoS attacks 

Malicious traffic 

 
 

Global Reach 

~216 Edge Locations (POPs) worldwide (at time of recording) 

Content available globally with minimal latency 

 
 

Key Takeaway 

CloudFront caches content at edge locations to deliver it faster and more securely to users worldwide. 

 

//////////////////////////////////// 

 

CloudFront Origins – What Are They? 

Origins are the source locations where CloudFront retrieves content from 

CloudFront caches this content at edge locations and delivers it to users 

 
 

1. S3 Bucket Origins 

Best for static content: 

Images 

Videos 

HTML, CSS, JS files 

Security 

Uses Origin Access Control (OAC) 

Replaces Origin Access Identity (OAI) 

Ensures only CloudFront can access the S3 bucket 

Prevents direct public access to S3 objects 

Extra Tip 

CloudFront can also act as an ingress to upload content to S3 

 
 

2. Custom Origins 

Used for HTTP-based backends, such as: 

Application Load Balancers (ALB) 

EC2 instances 

Any HTTP server 

S3 static website endpoints 
⚠️ Requires Static Website Hosting to be enabled on the bucket 

 
 

Key Differences 

Origin Type 

Use Case 

Security 

S3 Origin 

Static content 

OAC (CloudFront-only access) 

Custom Origin 

Dynamic / HTTP apps 

Security via ALB, SGs, WAF 

 
 

Exam Tip 🧠 

S3 + CloudFront + private bucket → OAC 

ALB / EC2 / HTTP backend → Custom Origin 

Static website S3 → Custom Origin (not S3 origin) 

 
 

One-Line Summary 

CloudFront origins define where your content lives—S3 for secure static content and custom origins for dynamic HTTP backends. 

//////////////////////////////////////////// 

 

CloudFront – High-Level Flow 

Client makes a request 

A user’s browser requests content (e.g. an image like image.jpg). 

Request goes to the nearest Edge Location (POP) 

CloudFront routes the request to the closest Edge location for lowest latency. 

Cache check at the Edge 

Cache hit ✅ 

Content already exists at the Edge 

CloudFront serves it immediately (very fast) 

Cache miss ❌ 

Content is not cached 

Fetch from the Origin 

CloudFront forwards the request to the origin: 

S3 bucket 

ALB / EC2 

Any HTTP-based backend 

Cache and deliver 

Origin returns the content to CloudFront 

CloudFront: 

Caches the content at the Edge 

Delivers it to the client 

Future requests 

Subsequent users near that Edge location get the content directly from cache 

 
 

Why This Is Fast 

Content is served from a location geographically close to users 

Reduces: 

Latency 

Load on origin servers 

Bandwidth costs 

 
 

One-Line Summary (Exam-Perfect) 

CloudFront accelerates content delivery by caching data at global Edge locations and serving users from the nearest cache instead of repeatedly hitting the origin. 

 
 

Key Exam Keywords to Remember 

Edge Location (POP) 

Cache hit vs cache miss 

Origin 

Latency reduction 

Global distribution 

 

//////////////////////////////// 

 

CloudFront with S3 as the Origin – How It Works 

1. S3 Bucket = Origin 

An S3 bucket stores your static content: 

Images 

Videos 

HTML, CSS, JS files 

This bucket is configured as the CloudFront origin. 

 
 

2. Global Edge Locations (POPs) 

CloudFront has edge locations worldwide 
(e.g. Los Angeles, Mumbai, São Paulo, Melbourne). 

These locations cache content close to users. 

 
 

3. User Request Flow 

A user requests a file (e.g. image.jpg) 

Request is routed to the nearest CloudFront edge location 

CloudFront checks its cache: 

Cache hit → file served immediately from the edge 

Cache miss → CloudFront fetches the file from the S3 bucket 

CloudFront: 

Returns the file to the user 

Caches it at the edge for future requests 

 
 

4. Security with Origin Access Control (OAC) 

The S3 bucket is private 

Only CloudFront is allowed to access it 

Enforced using: 

Origin Access Control (OAC) 

S3 bucket policy 

This prevents users from bypassing CloudFront and accessing S3 directly 

 
 

5. Why Use CloudFront + S3 

🚀 Faster delivery (content served from nearest edge) 

🔐 Better security (no public S3 access) 

📉 Reduced load & cost on the S3 origin 

🌍 Global scalability 

 

One-Line Exam Summary 

CloudFront with S3 as an origin delivers static content securely and efficiently by caching files at global edge locations while restricting direct access to the S3 bucket using Origin Access Control. 

 
 

Memory Tip 

“Edge first, S3 second — CloudFront only touches S3 if it has to.” 

 

///////////////////////// 

 

ALB / EC2 as CloudFront Origin – Key Notes 

1. Concept: 

CloudFront connects users to the closest edge location. 

Edge location forwards requests to the origin: either an ALB or EC2 instance. 

2. Public Access Requirements: 

ALB: Must be public to accept traffic from CloudFront. 

EC2 (if public): Must allow inbound traffic from CloudFront edge IPs. 

Security Group (SG) must permit only CloudFront public IPs. 

3. Architecture Options: 

Private EC2 behind ALB: 

ALB handles public requests. 

EC2 instances stay private and shielded. 

Public EC2 directly: 

EC2 handles requests from CloudFront. 

Must allow traffic from edge IPs. 

4. Security: 

Configure SG to only allow CloudFront IP ranges. 

Avoid exposing EC2 instances unnecessarily. 

5. Benefit: 

CloudFront adds speed and scalability while keeping the origin secure. 

 

//////////////////////////////// 

 

How Load Balancers Handle High Traffic (Black Friday Scenario) 

1. Entry Point – Route 53 (DNS) 

Acts as the front door of your app 

Routes users to the closest healthy region 

Reduces latency and avoids overloading a single endpoint 

 
 

2. Edge Caching – CloudFront 

Global CDN that caches static content (images, CSS, JS) 

Serves content close to users 

Greatly reduces load on backend systems 

Supports Lambda@Edge for: 

Modifying headers 

Lightweight request/response logic 

 
 

3. Traffic Distribution – Load Balancers 

CloudFront forwards requests to a Load Balancer 

Common choice for web apps: Application Load Balancer (ALB) 

ALB is used when you need: 

HTTP/HTTPS awareness 

Path-based routing (/api, /images) 

Host-based routing (api.example.com) 

WebSockets 

Advanced Layer 7 features 

Network Load Balancer (NLB): 

Used for high-performance, low-latency networking 

Layer 4 (TCP/UDP) 

No HTTP-level routing 

 
 

4. Backend Compute – Auto Scaling Groups (ASG) 

Load balancer targets live in an ASG 

ASG can scale: 

EC2 instances 

ECS / EKS containers 

Fargate tasks 

Automatically adds/removes capacity based on load 

 
 

5. Why This Scales on High Traffic Days 

Route 53 spreads users globally 

CloudFront absorbs most read/static traffic 

ALB distributes requests efficiently 

ASG scales compute automatically 

AWS handles the heavy lifting → site stays responsive 

 
 

Interview-Focused Concepts to Know 

When to choose ALB vs NLB 

Why connection draining is important 

Allows in-flight requests to finish during scale-in 

Auto-scaling metrics for read-heavy APIs 

CPU utilization 

Request count per target 

Latency 

 
 

One-Line Summary 

AWS handles massive traffic by combining DNS routing (Route 53), edge caching (CloudFront), smart load balancing (ALB/NLB), and auto-scaling compute, keeping apps fast and stable even at peak load. 

///////////////////////// 

 

AWS Networking – Core Concepts Recap 

Key Components to Understand 

VPC – isolated virtual network 

Subnets – public or private network segments 

Route Tables – control where traffic is sent 

Internet Gateway (IGW) – enables internet access 

NAT Gateway – outbound internet for private resources 

Security Groups (SGs) – instance-level firewall 

NACLs – subnet-level firewall 

 
 

“Set Up a VPC from Scratch” (Common Interview Question) 

To allow internet access, you need: 

Public Subnet 

Route table: 0.0.0.0/0 → Internet Gateway 

Used for: 

Load balancers 

Bastion hosts 

NAT Gateway 

Private Subnet 

Route table: 0.0.0.0/0 → NAT Gateway 

Used for: 

EC2, ECS, databases 

No direct inbound internet access 

 
 

Internet Gateway vs NAT Gateway 

Internet Gateway 

Inbound + outbound internet 

Required for public subnets 

NAT Gateway 

Outbound internet only 

Used by private subnets 

 
 

Stateful vs Stateless Firewalls (Very Important) 

Security Groups (Stateful) 

Instance-level 

Return traffic automatically allowed 

Allow rules only 

NACLs (Stateless) 

Subnet-level 

Inbound and outbound rules required 

Supports allow and deny rules 

Feature 

Security Group 

NACL 

Level 

Instance 

Subnet 

Stateful 

✅ 

❌ 

Rules 

Allow only 

Allow + Deny 

 
 

One-Line Interview Summary 

A VPC uses IGWs for public access, NAT gateways for private access, security groups as stateful firewalls, and NACLs as stateless firewalls, all connected through subnets and route tables. 

 
