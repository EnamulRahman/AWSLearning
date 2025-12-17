Amazon Route 53 — Introduction 

Amazon Route 53 is AWS’s fully managed DNS (Domain Name System) service. It's designed to be highly available, scalable, and globally reliable for routing user traffic to your AWS resources. 

 
 

What Is Route 53? 

Route 53 is an authoritative DNS service, meaning you have full control over DNS records for your domains. 
You can create, update, and delete DNS records to point traffic to: 

EC2 instances 

Load balancers 

CloudFront distributions 

S3 static websites 

Any IP or endpoint you manage 

Because it's authoritative, Route 53 acts as the source of truth for DNS queries related to your domain. 

 
 

Domain Registration 

Route 53 can also function as your domain registrar, similar to: 

GoDaddy 

Namecheap 

Cloudflare 

You can buy, transfer, and manage domain names directly in AWS. 

 
 

Health Checks 

Route 53 includes built-in health check functionality, allowing you to monitor your endpoints. 

Example: 
If your primary EC2 or on-prem server becomes unhealthy, Route 53 can automatically redirect traffic to a healthy resource. This makes it a key component for high availability architectures. 

 
 

100% SLA Uptime 

Route 53 is the only AWS service that offers a 100% availability SLA. 
DNS is critical to the internet, so Amazon designed Route 53 to be extremely resilient and globally distributed. 

 
 

Fun Fact — Why the name “Route 53”? 

Route 53 is named after port 53, the well-known port used for DNS queries. 
It’s a clever way of linking AWS’s DNS offering to classic networking fundamentals. 

 
 

Summary 

Amazon Route 53 is a powerful, enterprise-grade DNS service that: 

Manages DNS records 

Supports domain registration 

Performs health checks 

Routes users to healthy endpoints 

Provides industry-leading availability 

It’s deeply integrated with AWS, making it a foundational component for routing and directing traffic in cloud architectures. 

 

//////////////////////////////////////////// 

 

Route 53 — Hosted Zones (Public vs Private) 

What is a Hosted Zone? 

A container that stores all DNS records for a domain and its subdomains. 

Tells Route 53 how to route traffic for that domain. 

 
 

1. Public Hosted Zones 

Used for routing traffic on the public internet. 

Example: 

app1.mypublicdomain.com 

Any DNS record you create here is reachable from the internet. 

 
 

2. Private Hosted Zones 

Used for routing internal traffic within one or more VPCs. 

Ideal for internal services that shouldn’t be exposed publicly. 

Example: 

app1.company.internal 

Only resolvable by resources inside the VPC(s) you associate with it. 

 
 

3. Cost 

Approx. $0.50 per hosted zone per month. 

Important when managing many domains or multiple environments (dev/test/prod). 

 
 

Summary 

Public Hosted Zone: routes internet-facing traffic. 

Private Hosted Zone: routes internal VPC traffic only. 

Hosted zones are the core structure that Route 53 uses to manage DNS resolution and routing. 

 

///////////////////////////// 

 

 

Route 53 – Public vs Private Hosted Zones (Deep Dive) 

Public Hosted Zones 

Used for domains that must be accessible over the public internet. 

Route 53 resolves DNS queries from anyone on the internet. 

Example: 

example.com → resolves to a public IP (EC2, NLB, CloudFront, S3 static site, etc.) 

Ideal for: 

Websites 

Public APIs 

Load balancers 

Any public-facing service 

Function: 
Public query → Route 53 → Public IP of your resource. 

 
 

Private Hosted Zones 

Used for internal DNS resolution within one or more VPCs. 

DNS records are ONLY visible to resources inside the associated VPC(s). 

Example: 

api.example.internal 

db.example.internal 

Ideal for: 

Internal microservices 

Private APIs 

Databases 

Backend EC2 instances 

Function: 
Internal VPC query → Route 53 private zone → Private IP resolution (never exposed to internet). 

 
 

Key Differences 

Feature 

Public Hosted Zone 

Private Hosted Zone 

Visibility 

Internet-wide 

VPC-only 

Use case 

Websites, public APIs 

Databases, backend systems 

Traffic direction 

Resolves public IPs 

Resolves private IPs 

Security 

Public by design 

Secure & isolated 

 
 

Summary 

Public zones = internet-facing DNS. 

Private zones = internal, VPC-only DNS. 

Route 53 supports both, giving you a flexible, scalable DNS layer for any architecture—from public websites to highly secure internal services. 

 

 

////////////////////// 

 

 

 

⭐ DNS Fundamentals — Key Terms Explained 

1. Domain Registrar 

The service where you buy/register domain names. 

Examples: Route 53, Cloudflare, GoDaddy, Namecheap. 

Responsible for managing ownership of your domain. 

 
 

2. DNS Records 

Instructions that tell the internet where to route traffic for your domain. 

Common record types: 

A → maps domain → IPv4 address 

AAAA → maps domain → IPv6 address 

CNAME → alias to another domain 

NS → nameservers for your domain 

(Many more exist: MX, TXT, SRV, etc.) 

 
 

3. Zone File 

A container of all DNS records for your domain. 

A directory/map that defines how DNS should resolve your domain and subdomains. 

 
 

4. Nameserver (NS) 

The system that stores and serves DNS records for your domain. 

Answers DNS queries with authoritative information (from your zone file). 

Your domain registrar points your domain to these nameservers. 

 
 

5. TLD — Top-Level Domain 

The last part of a domain. 

Examples: .com, .org, .io, .gov, .net 

Highest level in the DNS hierarchy. 

 
 

6. SLD — Second-Level Domain 

The part of the domain you actually own/register. 

Examples: 

amazon in amazon.com 

google in google.com 

codercode in codercode.io 

 
 

⭐ Full URL Breakdown Example 

URL: http://api.www.example.com 

Components: 

Protocol: http:// 

Tells the browser how to communicate (Layer 7 / HTTP). 

FQDN (Fully Qualified Domain Name): api.www.example.com 

The complete domain path. 

Subdomains: api + www 

Represent different services or sections of a site. 

SLD (Second-Level Domain): example 

The name you own. 

TLD: .com 

Top-level domain in the DNS hierarchy. 

 
 

⭐ Summary 

DNS terms work together like this: 

Registrar → You buy a domain. 

Nameservers → Host your zone file. 

Zone file → Holds all your DNS records. 

DNS records → Tell the internet where to route traffic. 

TLD + SLD → Form your main domain. 

Subdomains → Organize your services (e.g., API, app, www). 

So when someone types a URL, DNS ensures they land exactly where they’re supposed to—quickly and reliably. 

 

/////////////////////////////////// 

 

 

 

⭐ How DNS Works — Step-by-Step (Simplified Notes) 

DNS = Domain Name System 
Purpose = Convert domain names → IP addresses (e.g., example.com → 9.10.11.12) 

 
 

1️⃣ Step 1 — User Enters Domain 

You type example.com into your browser. 

Browser asks: “What IP address belongs to this domain?” 

 
 

2️⃣ Step 2 — Query Goes to Local DNS Resolver 

This DNS resolver is usually your ISP or your company’s DNS server. 

It checks if it already has the answer cached. 

If not → it begins a recursive lookup. 

 
 

3️⃣ Step 3 — Local DNS Resolver Starts Asking the Global DNS Hierarchy 

Resolver acts like a person asking around for directions. 

 
 

4️⃣ Step 4 — Root DNS Servers 

First stop: Root DNS server (managed by ICANN). 

Root server does NOT know the IP address. 

But it knows which Top-Level Domain (TLD) server to ask. 

Example: 

Domain ends with .com → Root server says: 
“Go ask the .com DNS server.” 

 
 

5️⃣ Step 5 — TLD DNS Servers 

DNS resolver asks the TLD server (e.g., .com). 

TLD server doesn’t have the IP either. 

But it knows which authoritative nameserver (SLD DNS server) hosts DNS for example.com. 

 
 

6️⃣ Step 6 — Authoritative Nameserver (SLD DNS Server) 

This server is managed by your domain registrar (Route 53, GoDaddy, Cloudflare, etc.) 

It looks inside the zone file and finds the actual DNS record. 

Example result: 
example.com → 9.10.11.12 

 
 

7️⃣ Step 7 — DNS Resolver Returns IP to Browser 

Resolver caches the answer for a while (TTL). 

Sends the IP address back to your browser. 

 
 

8️⃣ Step 8 — Browser Connects to Web Server 

Now the browser knows the IP → 

It makes a real connection to the server hosting the website. 

 
 

⭐ DNS Process Summary (Super Simple) 

Step 

Who Is Asked? 

What Happens? 

1 

Browser → Local DNS 

“Where is example.com?” 

2 

Local DNS cache 

If found → return IP immediately 

3 

Root server 

Points to correct TLD server 

4 

TLD server (.com) 

Points to authoritative DNS server 

5 

Authoritative server 

Returns the actual IP address 

6 

Browser 

Connects to the web server 

 
 

⭐ Memory Trick (Very Easy) 

R → T → A → Browser 

Root → sends you to the right TLD 

TLD → sends you to the right Authoritative server 

Authoritative server → gives the IP 

Browser → loads the website 

 
 

⭐ One-Line Summary 

DNS is like asking for directions: 
Local DNS → Root → TLD → Authoritative → IP → Website loads — all in milliseconds. 

 

//////////////////////////////////// 

 

 

Route 53 – Records 

• How you want to route traffic for a domain 

• Each record contains: 

Domain/subdomain name – e.g., example.com 

Record type – e.g., A or AAAA 

Value – e.g., 12.34.56.78 

Routing Policy – how Route 53 responds to queries 

TTL – amount of time the record is cached at DNS resolvers 

 
 

• Route 53 supports the following DNS record types: 

Basic 

A 

AAAA 

CNAME 

NS 

Advanced 

CAA 

DS 

MX 

NAPTR 

PTR 

SOA 

TXT 

SPF 

SRV 

 

 

/////////////////// 

 

 

Route 53 – DNS Record Types (Core Basics) 

1. A Record 

Maps a hostname → IPv4 address. 

Example: 
example.com → 192.0.2.1 

Most common DNS record. 

 
 

2. AAAA Record (Quad-A) 

Same purpose as an A record, but for IPv6 addresses. 

Example: 
example.com → 2001:db8::1 

 
 

3. CNAME (Canonical Name) 

Maps one hostname to another hostname, not to an IP. 

It’s like giving a nickname. 

Example: 
www.example.com → example.com 

Important: 
❌ Cannot be used at the root domain (example.com).  
✔ Only for subdomains (www, api, app, etc.). 

 
 

4. NS Record (Name Server) 

Specifies the nameservers responsible for your hosted zone/domain. 

These tell the internet “who answers DNS questions for this domain.” 

Example: 
example.com → ns-123.awsdns-45.com 

 
 

Summary 

These simple DNS record types work together to help Route 53 route traffic correctly: 

Record 

Purpose 

A 

hostname → IPv4 

AAAA 

hostname → IPv6 

CNAME 

hostname → another hostname 

NS 

tells the internet which nameservers handle your domain 

 

//////////////////////////// 

 

Understanding TTL (Time To Live) in DNS 

TTL is simply how long a DNS record stays cached before a DNS resolver asks Route 53 for the updated value again. 

 
 

High TTL (Long Cache Time) 

Example: 24 to 

✅ Benefits 

Fewer DNS lookups → less cost in Route 53. 

Reduces load on DNS servers. 

❌ Drawbacks 

Slower propagation of DNS changes. 
If you update an IP address, users won’t see the change until the TTL expires. 

 
 

Low TTL (Short Cache Time) 

And or even 5 seconds 

✅ Benefits 

DNS changes propagate very quickly. 
Useful during: 

migrations 

server IP changes 

failovers 

❌ Dr. 

More frequent DNS queries → higher Route 53 cost. 

Slightly more load on DNS servers. 

 
 

Important Rule 

TTL is required for almost all DNS records except ALIAS records. 

Why? 

ALIAS records have no TTL because AWS manages caching and refresh automatically. 
(They essentially behave like a “smart CNAME” at the root level.) 

 
 

Summary 

Choosing TTL is a balance between: 

Priority 

Choose 

Lower cost 

High TTL 

Fast updates 

Low TTL 

It depends on whether you value cheap stable caching or rapid DNS updates. 

 

 

///////////////////////////////////////// 

 

🔹 CNAME vs ALIAS in Route 53 — Clear Breakdown 

1️⃣ Why do we need these records? 

AWS resources such as Load Balancers, CloudFront distributions, and API Gateways expose long AWS-generated hostnames, e.g.: 

my-loadbalancer-123456.us-east-1.elb.amazonaws.com 
 

You don’t want users typing that. 
So you create a friendly DNS name like: 

myapp.mydomain.com 
 

To map your domain to the AWS resource, you need either CNAME or ALIAS. 

 
 

🔹 CNAME Record 

What it does 

A CNAME maps one hostname → another hostname, for example: 

app.mydomain.com → my-loadbalancer-123.elb.amazonaws.com 
 

Limited 

❌ Cannot be used at the root domain 
You cannot set: 

mydomain.com → something.amazonaws.com   (INVALID) 
 

Root domains can only point to IPs or ALIAS records, not CNAMEs. 

Where CNAME can be used 

✅ Works for subdomains only: 

api.mydomain.com   
www.mydomain.com   
auth.mydomain.com   
 

 
 

🔹 ALIAS Record (AWS only — special feature) 

ALIAS records behave like a DNS “supercharged CNAME” that can be used at any level, including the root at home. 

What ALIAS can do 

✔ Points your domain to AWS resources like: 

Load Balancers 

CloudFront 

S3 website endpoints 

API Gateway 

VPC Interface Endpoints 

Example: 

mydomain.com → my-loadbalancer-123.elb.amazonaws.com  (VALID) 
 

Benefits 

✅ Allowed at the root domain 

✅ Free (no per-query charge like CNAME resolutions) 

✅ Supports AWS health checks 

✅ Looks like an A-record to DNS resolvers (no extra lookup) 

When to choose ALIAS 

Always prefer ALIAS when pointing to AWS services. 

 
 

🔥 Main Takeaway (Simple Rule) 

If you need to… 

Use 

Point a subdomain → hostname 

CNAME or ALIAS 

Point the root domain → AWS resource 

ALIAS only 

Get AWS integration, health checks, free queries 

ALIAS 

Point to any AWS service 

ALIAS 

ALIAS = AWS-optimized, free, root-compatible CNAME. 
CNAME = subdomains only. 

 

/////////////////////// 

 

🔹 Alias Records in Route 53 — Clear Explanation 

Alias records in Route 53 are a special AWS-only DNS feature designed to make integrating DNS with AWS resources simple and automatic. 

 
 

✔ What Alias Records Do 

Alias records map a domain name (like zoppo.com) directly to an AWS resource such as: 

Application Load Balancer (ALB) 

Network Load Balancer (NLB) 

CloudFront distribution 

S3 static website endpoint 

API Gateway 

VPC endpoint 

Global Needles 

They behave like A or AAAA records, not CNAMEs. 

 
 

✔ Why Alias Records Are Better Than CNAME 

1️⃣ No problem with root domains 

CNAME cannot be used at the root domain: 

❌ 'zoppo.com → something.ama 

Alias records can: 

✅ 'zoppo.com → my-alb-123.elb.amazonaws.c 

This is called zone apex or root domain support. 

 
 

2️⃣ Outs 

AWS resources often change underlying IPs (especially load balancers). 
With Alias records: 

No manual updates needed 

Route 53 automatically follows AWS infrastructure changes 

This makes them extremely reliable. 

 
 

3️⃣ Alias ≠ CNAME 

A CNAME always maps hostname → hostname. 

Alias maps: 

hostname → AWS resource 
 

It behaves like an A/AAAA record in DNS resolution, not a CNAME. 

 
 

4️⃣ TTL Can't Be Set 

You cannot set TTL for alias records. 
AWS automatically manages TTL behind the scenes to optimise routing and health. 

This ensures fast propagation of AWS-managed changes. 

 
 

✔ Technical Form: A or AAAA Only 

Alias records must be: 

A (IPv4) 

AAAA (IPv6) 

Because they essentially behave like IP-mapping records. 

No CNAME for alias, because alias already replaces that functionality in AWS. 

 
 

⭐ Summary (Easy to Remember) 

Alias Record = A supercharged CNAME made for AWS. 

Works at root domain 👉 CNAME cannot 

Tracks AWS IP changes automatically 

Used for AWS resources (ALB, CloudFront, S3, API Gateway, etc.) 

TTL is managed by AWS (you don’t set it) 

Only A / AAAA types 

Alias ≈ CNAME + automation + AWS integration + root-domain support 

 

//////////////////////////////////// 

 

 

Route 53 – Alias Record Targets (Where You Can Use Alias Records) 

Alias records only work with specific AWS-managed resources — not everything supports them. 

 
 

✅ Common Alias Record Targets 

1. Elastic Load Balancers (ALB & NLB) 

Most common use case 

Map domain → load balancer hostname 

Alias automatically tracks changing IPs 

 
 

2. CloudFront Distributed 

Needed for custom domains 

Alias avoids extra DNS lookups and is free 

 
 

3. API Gateway 

Use alias to map your domain to API Gateway’s hostname 

Cleaner URLs for APIs 

 
 

4. Elastic Beanstalk Environments 

Point your domain directly to the Beanstalk environment endpoint/load balancer 

 
 

5. S3 Static Website Hosting 

Use alias to map domain → S3 website endpoint 

Great for static sites 

 
 

6. VPC Interface Endpoints (PrivateLink) 

Alias provides easy internal DNS for private AWS services 

 
 

7. AWS Global Accelerator 

Map your domain to a Global Accelerator DNS name for global low-latency routing 

 
 

8. Other Route 53 Records (same hosted zone) 

Alias can point to another record inside the same zone 

Example: app.example.com → api.example.com 

 
 

❌ Where Alias Records CANNOT Be Used 

EC2 DNS instance n 

You cannot set an alias record pointing to: 

ec2-123-45-67-89.compute.amazonaws.com 
 

For EC2 use: 

A record (to map directly to an IP), or 

CNAME (for subdomains only) 

 
 

⭐ Key Takeaway 

Alias records = AWS-optimised, free, auto-updating CNAME replacement 
Best for AWS services whose underlying IPs change frequently. 

 

////////////////////////////////////////////////////// 

 

Route 53 – Routing Policies (DNS-level routing) 

Important: Route 53 does not route traffic like a load balancer. 
It only decides which DNS response (IP/endpoint) to return. 

 
 

1. Simple Routing 

Always returns the same IP or endpoint 

No health checks or logic 

Best for single server setups 

 
 

2. Weighted Routing 

Split traffic by percentage 

Example: 

Server A → 70% 

Server B → 30% 

Useful for A/B testing or gradual rollouts 

 
 

3. Failover Routing 

Primary + secondary setup 

Uses health checks 

If primary fails → traffic goes to backup 

Ideal for high availability 

 
 

4. Latency-Based Routing 

Routes users to the lowest-latency endpoint 

Great for multi-region applications 

Improves user experience globally 

 
 

5. Geolocation Routing 

Routes traffic based on user’s location 

Example: 

Europe → EU servers 

US → US servers 

Useful for compliance or region-specific content 

 
 

6. Multi-Value Answer Routing 

Returns multiple IP addresses 

Route 53 checks health and removes unhealthy IPs 

Lightweight alternative to a load balancer 

 
 

7. Geoproximity Routing 

Routes based on location of users AND resources 

Can bias traffic toward specific regions 

Configured using Route 53 Traffic Flow 

More flexible than geolocation 

 
 

⭐ Key Takeaway 

Route 53 routing policies control DNS responses, not traffic flow itself. 
They help with availability, performance, testing, and geographic control. 

 

\\\\\\\\\\\\\\\\\\\\\ 

 

Domain Registrar vs DNS Service 

Domain Registrar 

Where you buy and own a domain name (e.g. example.com) 

You pay an annual fee to keep ownership 

Examples: 

GoDaddy 

Amazon Registrar 

Cloudflare Registrar 

Controls domain ownership, not traffic routing 

👉 Think of it as the land registry for your domain 

 
 

DNS Service (DNS Provider) 

Manages DNS records for the domain 

Converts domain names into IP addresses 

Controls where traffic goes 

Examples: 

Amazon Route 53 

Cloudflare DNS 

Google DNS 

👉 Think of it as the phone book for your domain 

 
 

Key Point: They Are Independent 

You can: 

Buy a domain from GoDaddy 

Use Route 53 for DNS 

You do this by changing the nameservers at the registrar 

 
 

Typical Flow 

Buy domain from a registrar 

Create a hosted zone in Route 53 

Copy Route 53 NS records 

Update nameservers at the registrar 

Route 53 now controls DNS 

 
 

Why Use Route 53 for DNS? 

AWS integration (ALB, CloudFront, S3, etc.) 

Advanced routing policies 

Health checks 

High availability (100% SLA) 

 
 

⭐ Exam Tip 

Registrar = ownership 
DNS service = routing & resolution 

 

//////////////////////////// 

 

Using a 3rd-Party Registrar with Amazon Route 53 

Scenario 

You buy a domain from a 3rd-party registrar: 

GoDaddy 

Namecheap 

Cloudflare 

etc. 

You want Amazon Route 53 to manage your DNS instead of the registrar’s DNS. 

 
 

Step-by-Step Process 

1️⃣ Create a Hosted Zone in Route 53 

Create a Public Hosted Zone 

This hosted zone will store all DNS records for your domain 

Route 53 automatically creates: 

NS (Name Server) records 

SOA record 

 
 

2️⃣ Get Route 53 Name Servers 

Route 53 provides 4 authoritative name servers 

Example format: 

ns-123.awsdns-45.com 

ns-456.awsdns-67.net 

These tell the internet: 

“Route 53 is authoritative for this domain” 

 
 

3️⃣ Update Name Servers at the Registrar 

Go to your registrar (GoDaddy / Namecheap / etc.) 

Replace existing name servers with the Route 53 name servers 

Save changes 

⏱ DNS propagation can take minutes to hours (sometimes up to 48 hours) 

 
 

4️⃣ Route 53 Now Controls DNS 

All DNS queries now go to Route 53 

You manage: 

A / AAAA records 

CNAME 

ALIAS 

Routing policies 

Health checks 

 
 

Key Concept (EXAM GOLD) 

Registrar = domain ownership 
Route 53 = DNS resolution & routing 

They are independent services 

 
 

Why Use Route 53 Instead of Registrar DNS? 

ALIAS records (root domains + AWS resources) 

Health checks & failover 

Advanced routing policies 

Deep AWS integration 

100% SLA 

 
 

Common Exam Trap 

❌ “You must buy the domain from AWS to use Route 53” 
✅ False — Route 53 works with any registrar 

 
 

One-Line Summary 

Buy domain anywhere → point name servers to Route 53 → Route 53 handles DNS 

 

////////////////// 

 

 
