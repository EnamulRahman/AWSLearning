Scalability vs High Availability (HA) — Key Points 

Scalability 

Ability of a system to handle increased load. 

Vertical scalability: Add more power to one machine (CPU/RAM upgrade). 

Horizontal scalability (elasticity): Add more machines/instances to share load. 

High Availability (HA) 

Ensures the system stays up and running even if components fail. 

Focuses on redundancy and eliminating single points of failure. 

Relationship 

Scalability = handle more demand. 

High availability = stay operational during failures. 

A system can be highly available but not scalable, and vice versa. 

Analogy 

Vertical scaling → giving one worker better tools. 

Horizontal scaling → hiring more workers. 

HA → having backup workers in case one is unavailable. 

 

 

Load Balancing — Key Points 

What is Load Balancing? 

Distributes incoming traffic across multiple servers/EC2 instances. 

Prevents a single instance from becoming overloaded. 

Improves performance and keeps applications responsive. 

Analogy 

One chef vs multiple chefs with a manager (Load Balancer) directing orders. 

 
 

How Elastic Load Balancer (ELB) Works 

Sits between users and EC2 instances. 

Forwards requests to healthy instances. 

Continuously performs health checks. 

If an instance fails, traffic is automatically redirected to healthy ones. 

Helps maintain availability and fault tolerance. 

 
 

Reverse Proxy — Quick Definition 

Similar to a load balancer but with extra capabilities: 

Content-based routing 

Path/host-based routing 

Directing traffic to different services/microservices 

AWS ALB (Application Load Balancer) includes reverse proxy features. 

 
 

Why Load Balancing Matters 

Enables scalability as traffic grows. 

Increases resilience by removing failed instances automatically. 

Ensures smooth, uninterrupted user experience. 

 

// 

 

Health Checks — Key Points 

What Are Health Checks? 

Mechanism that lets the load balancer verify if instances are healthy and able to respond. 

Prevents traffic from being sent to down or malfunctioning instances. 

 
 

How Health Checks Work 

Load balancer sends requests to a specific port + route (e.g., /health). 

Instance responds with: 

200 OK → Healthy 

Anything else (no response, error) → Unhealthy 

Unhealthy instances are removed from traffic rotation until they recover. 

 
 

Why Health Checks Matter 

Ensure users are only routed to working, responsive instances. 

Enable automatic failover—traffic moves to healthy instances without manual action. 

Critical for maintaining good user experience in multi-instance architectures. 

 
 

Example Scenario 

One instance becomes unresponsive (e.g., high CPU). 

Health check marks it unhealthy. 

Load balancer stops sending traffic to it and uses healthy instances instead. 

When the instance recovers and passes health checks, it is re-added. 

 

AWS Load Balancers — Key Points 

1. Classic Load Balancer (CLB) 

Old generation (2009). 

Supports HTTP, HTTPS, TCP, SSL. 

Simple but limited; lacks modern features. 

Rarely used today. 

 
 

2. Application Load Balancer (ALB) — Layer 7 

Released 2016. 

For HTTP, HTTPS, WebSocket traffic. 

Content-based routing (URLs, headers, params, cookies). 

Ideal for modern web apps, microservices, containers. 

Smarter routing logic at the application layer. 

 
 

3. Network Load Balancer (NLB) — Layer 4 

Released 2017. 

Handles TCP/UDP/HTTP with ultra-low latency. 

Designed for high performance and millions of requests/sec. 

Best for real-time, high-throughput, low-latency workloads (e.g., gaming, trading systems). 

 
 

4. Gateway Load Balancer (GWLB) — Layer 3 

Works at the network layer (IP). 

Designed to deploy and scale network appliances: 

Firewalls 

IDS/IPS 

Traffic inspection systems 

Used for advanced networking setups. 

 
 

Which Load Balancer to Choose? 

ALB → Traditional web apps, microservices, content-based routing. 

NLB → High-volume, low-latency TCP workloads. 

GWLB → Network security appliances and advanced routing. 

CLB → Legacy use only (not recommended for new systems). 

 
 

Other Notes 

Load balancers can be internal (private) or external (public). 

AWS recommends using ALB, NLB, or GWLB over CLB. 

 

 

Application Load Balancer (ALB) — Key Notes 

What is an ALB? 

A modern AWS load balancer that operates at Layer 7 (HTTP/HTTPS). 

Understands application-level content: 

Headers 

Cookies 

URLs 

Query strings 

 
 

Key Features 

1. Smart, Content-Based Routing 

Routes traffic based on request content. 

Supports multiple target groups. 

Can route to: 

Multiple applications on different instances 

Multiple applications on the same instance (common in container setups like ECS) 

2. Designed for Modern Apps 

Ideal for microservices, containerised workloads, and web apps needing advanced routing. 

3. Protocol Support 

HTTP/1.1 

HTTP/2 (better performance, efficient connections) 

WebSockets (real-time communication, e.g., chat apps, live dashboards) 

 
 

Why ALB Is a Game Changer 

Provides flexible, intelligent routing for complex architectures. 

Enables hosting many apps behind one load balancer. 

Supports real-time and modern web protocols out of the box. 

 

 

Application Load Balancer (ALB) — HTTP Traffic Handling 

How ALB Handles Traffic 

Users send HTTP/HTTPS requests to an external ALB. 

ALB examines each request and forwards it to the correct target group based on routing rules. 

 
 

Target Groups 

Contain backend resources: EC2, ECS tasks, Lambda, etc. 

Each target group typically represents a specific service: 

e.g., user service (login/profile) 

e.g., search service 

Allows clean separation of application components → ideal for microservices. 

 
 

Routing Logic 

ALB can route based on path, host, query, headers, etc. 

/user/* → user-related target group 

/search/* → search target group 

Enables running multiple services behind one load balancer. 

 
 

Health Checks 

ALB performs continuous health checks on each target. 

If an instance fails: 

Marked unhealthy 

ALB stops routing to it 

Traffic is forwarded only to healthy instances 

 
 

Benefits 

Clean separation of services → no interference between components. 

Better scaling per service (each target group scales independently). 

Supports modern web architectures using HTTP/HTTPS. 

 

 

Application Load Balancer (Target Groups) 

EC2 instances (can be managed by an Auto Scaling Group) – HTTP 

ECS tasks (managed by ECS itself) – HTTP 

Lambda functions – HTTP request is translated into a JSON event 

IP Addresses – must be private IPs 

ALB can route to multiple target groups 

Health checks are at the target group level 

 

 

Network Load Balancer (NLB) — Key Notes 

What is an NLB? 

Operates at Layer 4 (TCP/UDP). 

Designed for extreme performance, low latency, and very high traffic volumes. 

Can handle millions of requests per second. 

 
 

Key Benefits 

Ultra-low latency (~100 ms vs ~400 ms for ALB). 

Static IP per Availability Zone 

Useful for fixed endpoints and security rules. 

Can attach Elastic IPs. 

Ideal for: 

High-throughput workloads 

Real-time systems 

Financial trading apps 

Gaming servers 

DNS/VoIP or other TCP/UDP-based protocols 

 
 

Important Notes 

Not included in the AWS free tier → expect costs. 

Best option when applications require: 

Very fast response times 

Consistent network performance 

Protocol support beyond HTTP (e.g., TCP/UDP) 

 
 

Example Use Case 

Real-time trading platform 
→ NLB ensures minimal delay and handles huge traffic spikes reliably. 

 

Network Load Balancer (NLB) — TCP / Layer 4 Traffic Handling 

What is Layer 4 Traffic? 

Layer 4 = Transport layer (TCP/UDP). 

NLB routes raw TCP/UDP traffic, without looking at HTTP headers or content. 

 
 

How NLB Handles Traffic 

Forwards traffic based on ports and protocols (TCP/UDP). 

Routes requests to target groups depending on rules. 

Suitable for: 

Pure TCP apps 

Mixed workloads where HTTP runs inside TCP 

Real-time or performance-sensitive services 

 
 

Key Characteristics 

No Layer 7 inspection 

Does not read HTTP headers 

Does not perform SSL termination 

Does not modify traffic 

Focuses on speed, efficiency, and minimal latency. 

 
 

Use Cases 

High-performance TCP/UDP applications, e.g.: 

Gaming servers 

Streaming services 

Real-time apps 

DNS/VoIP 

Any workload needing very low latency 

Different applications can be routed to different TCP-based target groups. 

 
 

When Not to Use NLB 

If you need: 

Content-based routing 

SSL/TLS termination 

HTTP header inspection 
→ Use Application Load Balancer (ALB) instead. 

 
 

Summary 

NLB forwards TCP/UDP traffic at Layer 4 with minimal overhead. 

Handles millions of requests with extremely low latency. 

Ideal for performance-critical, scalable network applications. 

 

---------------- 

 

Sticky Sessions (Session Affinity) — Key Notes 

What Are Sticky Sessions? 

Ensure a user/client is always routed to the same backend instance. 

Useful when user session data is stored locally on one instance and not shared. 

Works with CLB, ALB, and NLB. 

 
 

How They Work 

Implemented using cookies: 

Load balancer issues a cookie identifying the instance. 

User keeps being directed to that same instance. 

Cookie expiration controls how long the stickiness lasts. 

 
 

When to Use Sticky Sessions 

Apps with session-specific data stored on one instance, e.g.: 

Shopping carts 

Login sessions 

Temporary data not stored in shared storage 

Prevents issues like lost carts/session resets. 

 
 

Trade-Offs / Drawbacks 

Can cause traffic imbalance: 

Too many clients may stick to one instance. 

Reduces load balancing efficiency. 

Should be used only when required for app functionality. 

 
 

Key Considerations 

Good for session-heavy apps. 

Avoid if performance and even load distribution are more important. 

Stickiness behaviour can be fine-tuned via cookie settings. 

 

 

----------------------- 

 

SSL/TLS Certificates — Key Notes 

What Are SSL/TLS Certificates? 

Provide encryption in transit between client (browser) and server. 

Prevent data snooping or tampering → ensures secure communication. 

Common term “SSL” is outdated; modern systems actually use TLS. 

 
 

SSL vs TLS 

SSL (Secure Socket Layer) → older protocol. 

TLS (Transport Layer Security) → newer, more secure version (essentially “SSL 2.0”). 

People still say “SSL certificates,” but they are really TLS certificates. 

 
 

Who Issues Certificates? (Certificate Authorities – CAs) 

Trusted third parties that verify website identity. 

Examples: 

DigiCert 

GlobalSign 

GoDaddy 

Comodo 

Symantec 

Let’s Encrypt (free and widely used) 

 
 

Expiration & Renewal 

Certificates expire (1–3 years typically, sometimes up to 10). 

Must be renewed to avoid browser warnings and trust issues. 

Tools like CertManager can automate renewal. 

 
 

Key Takeaways 

TLS = modern, secure replacement for SSL. 

Certificates ensure encrypted, secure communication. 

Issued by trusted Certificate Authorities. 

Always renew before expiry to maintain trust and avoid warnings. 

 

////////////////////// 

 

SSL/TLS on Load Balancers — Key Notes 

How Load Balancers Use Certificates 

Load balancer acts as the secure middleman between user and EC2 instances. 

Uses X.509 certificates (SSL/TLS certificates) to encrypt traffic. 

Certificates are managed through AWS Certificate Manager (ACM). 

Can create, renew, or import custom certificates. 

 
 

HTTPS Listeners 

Used to enable encrypted connections on the load balancer. 

You must configure: 

A default certificate (used when no rule matches). 

Optional multiple certificates for multi-domain apps. 

 
 

SNI (Server Name Indication) 

Allows clients to specify which hostname they want. 

Load balancer selects the correct certificate for that domain. 

Enables support for multiple domains on one load balancer. 

 
 

Security Policies 

Control which SSL/TLS versions and ciphers are allowed. 

Helps support legacy clients if needed. 

 
 

Traffic Flow 

User connects to the load balancer over HTTPS → encrypted. 

Load balancer terminates SSL/TLS. 

Inside the VPC, LB forwards request to EC2 using HTTP (private network, encryption optional). 

EC2 instance processes the request. 

 
 

Why This Matters 

Ensures secure communication over the internet. 

Offloads encryption from EC2 instances → simpler, more efficient architecture. 

Centralizes certificate management via ACM. 

 

//////////////////////////////////////////////////////// 

 

 
 

Server Name Indication (SNI) — Key Notes 

What Problem Does SNI Solve? 

Before SNI: 

To host multiple HTTPS websites on one server, you needed a separate IP address for each certificate. 

With SNI: 

A single server/load balancer can host multiple SSL/TLS certificates for different websites. 

 
 

How SNI Works 

During the SSL/TLS handshake, the client sends the hostname it wants to access. 

The server/load balancer selects the correct certificate for that hostname. 

If no match is found → uses the default certificate. 

 
 

Why SNI Is Important 

Enables hosting multiple HTTPS sites on the same: 

Server 

ALB 

NLB 

CloudFront distribution 

Saves IP addresses and simplifies certificate management. 

 
 

Compatibility 

Supported: 

Application Load Balancer (ALB) 

Network Load Balancer (NLB) 

CloudFront 

Not supported: 

Classic Load Balancer (CLB) 

 
 

Summary 

SNI = multiple certificates, one load balancer. 

Eliminates the need for multiple IPs. 

Essential for modern multi-domain hosting. 

 

 

//////////////////////////////////////////////////// 

 

SSL Certificates Across ELB Types — Key Notes 

1. Classic Load Balancer (CLB) 

Only 1 SSL certificate supported. 

Cannot host multiple HTTPS domains on one CLB. 

Requires multiple CLBs for multiple certificates → costly & inflexible. 

Does NOT support SNI. 

 
 

2. Application Load Balancer (ALB) 

Supports multiple SSL/TLS certificates on a single LB. 

Uses SNI to serve the correct certificate per hostname. 

Allows hosting multiple domains on one ALB. 

Much more flexible and cost-efficient. 

 
 

3. Network Load Balancer (NLB) 

Also supports multiple SSL certificates using SNI. 

Useful when you need Layer 4 performance + TLS termination. 

Ideal for TCP/UDP workloads that still require encryption. 

 
 

Key Takeaway 

ALB and NLB (new generation) → Flexible SSL management, multiple certificates, SNI support. 

CLB (legacy) → One certificate only; no SNI; poor fit for multi-domain HTTPS. 

Upgrading to ALB/NLB is recommended for modern SSL/TLS requirements. 

 

 

////////////////////////////// 

 

Connection Draining 

Feature Naming 

Connection Draining – for CLB 

Deregistration Delay – for ALB & NLB 

Time allowed to complete in-flight requests while the instance is deregistering or unhealthy. 

Stops sending new requests to the EC2 instance that is deregistering. 

Configurable between 1 to 3600 seconds (default: 300 seconds). 

Can be disabled (set value to 0). 

Set to a low value if your requests are short. 

 

/////////////////////////////////////////// 

 

 

 

////////////////////////////// 

 

Auto Scaling Group (ASG) — Key Attributes 

Launch Templates 

Modern replacement for launch configurations. 

Ensure new instances launched by the ASG use consistent configuration. 

 
 

Core Attributes Inside Launch Template 

AMI: Pre-configured Amazon Machine Image; ensures identical setup for each instance. 

Instance Type: Defines compute, memory, storage capabilities (e.g., T2, compute-optimised, memory-optimised). 

EC2 User Data: Optional startup scripts (install software, configure environment). 

EBS Volumes: Persistent storage attached to instances. 

Security Groups: Virtual firewalls controlling inbound/outbound traffic. 

SSH Key Pair: Enables secure SSH access. 

IAM Role: Grants the instance permissions to access AWS services without storing keys. 

Network/Subnets: Specifies the VPC and subnets where instances will launch. 

Load Balancer Info: New instances automatically registered to ALB/NLB if configured. 

 
 

ASG-Specific Attributes 

Minimum Size: Lowest number of instances. 

Maximum Size: Highest number allowed. 

Desired/Initial Capacity: Start-up number of instances. 

Scaling Policies: Rules that determine when to scale in/out based on performance or traffic. 

 
 

Key Benefit 

Ensures consistent, automated, and scalable EC2 instance deployment as demand changes. 

 

///////////////////////// 

 

 
