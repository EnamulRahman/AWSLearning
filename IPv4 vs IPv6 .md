🌐 Layer 3 Routing, NAT & Internet Gateways — Notes 

🗂️ Layer 3 Routers (Networking Module Recap) 

Operate at Layer 3 (Network Layer) of the OSI model. 

Responsible for routing traffic between different networks. 

Use IP addresses and routing tables to decide where packets go. 

 
 

🏢 Private Networks (Company A & Company B) 

Both companies have their own private networks using private IP ranges 
(e.g., 192.168.x.x, 10.x.x.x, 172.16–31.x.x). 

Private IPs are NOT routable on the internet. 

They cannot directly communicate with external services. 

They require translation to a public IP to reach the internet. 

 
 

🌍 How They Access the Internet 

🔗 Internet Gateway (IGW) 

Acts like a router that connects a private network to the public internet. 

Similar to your home router: 

Your home devices use private IPs like 192.168.0.10. 

The router translates this into a single public IP when you browse online. 

💡 Key Function: NAT (Network Address Translation) 

NAT converts: 

Private → Public IP when sending traffic to the internet. 

Public → Private IP when receiving responses. 

Used by: 

Home routers 

Company routers 

Cloud internet gateways (AWS IGW) 

 
 

🔁 Why NAT Matters 

Allows many private devices to share one public IP. 

Keeps private networks hidden from the public internet. 

Enables communication with external services without exposing internal IPs. 

 
 

📌 Why the Internet Gateway Is Essential 

Without an IGW (or router at home): 

Private networks remain isolated. 

Company A & B cannot access websites or external services. 

No communication between private and public networks. 

 
 

🌐 Public vs Private IPs 

Public IPs 

Globally unique 

Identifies a device/network on the internet 

Assigned by ISPs or cloud providers 

Private IPs 

Can be reused across different internal networks 
(Company A and B can both use 10.0.0.0/16 with no conflict). 

Only usable within private/local networks. 

 
 

🔎 Summary 

Routers (Layer 3) move traffic between networks. 

Private networks need a router/IGW to reach the public internet. 

NAT enables private IPs to communicate externally using public IPs. 

Internet Gateways in AWS behave just like your home router— 
translating traffic and enabling secure access to the internet. 

 

 

🌐 Public vs Private IP Addresses — Notes 

🔵 Public IP Addresses 

What is a Public IP? 

An IP address directly reachable on the internet. 

Used by devices/services that need to be visible externally 
(e.g., website servers, cloud instances, your home router’s external IP). 

Key Characteristics 

Globally unique 

No two devices on the public internet can share the same public IP at the same time. 

Internet-routable 

Traffic can flow across the global internet to this address. 

Geolocatable 

Websites/services can typically identify your country or city based on your public IP. 

This is how websites show local content or targeted ads. 

Assigned by ISPs / Cloud Providers 

Not chosen manually, usually allocated automatically. 

 
 

🟢 Private IP Addresses 

What is a Private IP? 

An IP address used inside a private network like: 

Your home Wi-Fi 

A company LAN 

A VPC inside AWS 

Devices can communicate internally but cannot be reached directly from the internet. 

Key Characteristics 

Unique only within that private network 

Must not clash within the same network, but can be reused across different networks. 

Not internet-routable 

Requires NAT or an Internet Gateway to reach the internet. 

Reused everywhere 

Multiple networks worldwide can use identical private IP ranges with no conflict. 

Example: 

Your home may use 192.168.0.1 

My home may also use 192.168.0.1 

Company A and Company B can both use 192.168.0.1 

No issues because these networks are isolated. 

Private IP Ranges (RFC 1918) 

10.0.0.0 – 10.255.255.255 

172.16.0.0 – 172.31.255.255 

192.168.0.0 – 192.168.255.255 

 
 

🔍 Summary 

Feature 

Public IP 

Private IP 

Internet-routable 

✅ Yes 

❌ No 

Must be globally unique 

✅ Yes 

❌ No (unique only inside local network) 

Used by 

Servers, routers, cloud instances 

Home devices, company networks, VPCs 

Can be geolocated 

✅ Yes 

❌ No 

Requires NAT to reach internet 

❌ No 

✅ Yes 

Can be reused across networks 

❌ No 

✅ Yes 

 

 Elastic IPs 

 

🚀 What Is an Elastic IP? 

An Elastic IP Address (EIP) is a static, public IPv4 address that you can allocate to your AWS account and attach to a resource—usually an EC2 instance or a NAT Gateway. 

Instead of changing every time your instance stops/starts, an EIP stays the same until you release it. 

 
 

🧠 Why Do We Use Elastic IPs? 

✔ 1. Static Public IP 

A normal EC2 public IP changes whenever you stop/start the instance. 
An Elastic IP does not change, which is important for: 

Hosting a website 

Running an API 

SSH/RDP access without updating IPs 

DNS records that need a fixed IP 

✔ 2. Re-attach Quickly 

If one instance fails, you can detach the EIP and attach it to another instance in seconds. 
This improves fault tolerance. 

 
 

📌 Key Facts You MUST Remember 

🔹 You pay for Elastic IPs when they’re NOT used. 

You can have one Elastic IP attached to a running instance for free, but AWS charges you when: 

The EIP is allocated but not attached 

The EIP is attached, but the instance is stopped 

This encourages users not to hoard static IPs. 

🔹 One EIP per instance by default 

Although you can have multiple, you must make network adjustments. 

🔹 Region-specific 

An Elastic IP is tied to a region, not an availability zone. 

 

 

❗ Why You Should Avoid Elastic IPs (Best Practice) 

Modern AWS architecture encourages NOT using Elastic IPs unless essential. 

Here’s why: 

1. They Indicate Legacy or Poor Architecture 

Needing many static IPs often means: 

No load balancing 

No DNS routing 

Single-instance dependency 

Manual failover processes 

This goes against cloud-native principles. 

2. Better Alternatives Exist 

Option A — Public IP + DNS 

Use a normal public IP (which changes) and map it to a Domain Name (Route 53). 
If the instance IP changes, update DNS → client still uses the domain. 

Option B — Elastic Load Balancer (ELB) 

Load balancers: 

Don’t need Elastic IPs 

Automatically distribute traffic 

Provide health checks 

Are highly available 

Handle SSL/TLS termination 

This is the recommended solution for modern scalable apps. 

3. Charges Apply When You Aren’t Using Them 

AWS charges you if an Elastic IP is allocated but not attached to a running instance. 
This prevents users from hoarding IPv4s. 

4. Strict Limit 

Default: 5 EIPs per AWS account per region. 
You can request more, but AWS wants you to avoid them. 

 
 

🚫 When Not to Use Elastic IPs 

Do NOT use an Elastic IP if: 

You’re using a Load Balancer 

Your architecture scales dynamically (Auto Scaling Group) 

You only need stable connectivity — DNS can solve this 

You want fully managed high availability 

 
 

🧠 Extra Useful Details (Not in Transcript) 

Elastic IPs are IPv4 only 

IPv6 has enormous address space → no need for static allocation. 

Elastic IPs are regional resources 

Cannot move an EIP across regions. 

Remapping takes seconds, DNS changes can take minutes 

This is why EIPs can still be useful for very strict failover needs. 

EIPs can be associated with: 

EC2 instances 

Network interfaces (ENIs) 

NAT Gateways (automatically use EIPs) 

Security Note 

EIPs still require: 

Security Groups 

NACL rules 

They only provide an address — not access. 

 

 

 
