Backbone of network security 

 

AWS Security Groups — Key Notes 

General Characteristics 

Security Groups (SGs) can be attached to multiple EC2 instances (reusable). 

SGs are tied to a specific Region + VPC. 

Cannot use an SG created in one region/VPC in another. 

SGs live outside the EC2 instance. 

They act as a virtual firewall before traffic reaches the instance. 

 
 

Behaviour & Best Practices 

Inbound & Outbound Rules 

Inbound traffic: BLOCKED by default 
→ Must explicitly allow ports (SSH, HTTP, HTTPS, etc.) 

Outbound traffic: ALLOWED by default 
→ Instances can reach the internet/other instances unless restricted. 

Troubleshooting Patterns 

Timeouts = almost always an SG issue 
→ Traffic blocked before reaching the EC2 instance. 

Connection refused = app/instance issue 
→ The service isn't running or is misconfigured, not an SG problem. 

Best Practice 

Use a separate SG for SSH access. 

Allows tight control of SSH IP ranges. 

Avoids mixing with web/app rules. 

 
 

Key Takeaways 

SGs are reusable across instances. 

Region + VPC–specific. 

Block inbound by default; allow outbound by default. 

First place to check when apps are unreachable. 

Separate SGs for SSH is recommended. 

 

 

 

 
