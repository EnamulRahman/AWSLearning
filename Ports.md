💻 Common & Important Network Ports (for AWS Security Groups & Networking) 

🔐 SSH – Port 22 

Purpose: Securely log into Linux servers (remote command-line access). 

Use Case: Managing EC2 Linux instances. 

Important: 

Always restrict port 22 to trusted IPs only (your IP). 

Do not leave it open to the world (0.0.0.0/0). 

📁 FTP – Port 21 

Purpose: File Transfer Protocol (uploading/downloading files). 

Notes: 

Not secure (transfers are unencrypted). 

Being phased out for secure alternatives. 

🔐 SFTP – Port 22 

Purpose: Secure File Transfer Protocol. 

Why same port as SSH? 

SFTP runs over SSH, providing encryption. 

Preferred option for secure file transfers. 

🌐 HTTP – Port 80 

Purpose: Accessing unsecured websites. 

Notes: 

No encryption → data visible in plain text. 

Often used for public web servers, redirects, or temporary setups. 

🔒 HTTPS – Port 443 

Purpose: Accessing secure websites (TLS/SSL encrypted). 

Notes: 

Industry standard for secure web traffic. 

Required for login pages, ecommerce, APIs, apps handling sensitive data. 

🌍 DNS – Port 53 

Purpose: Domain Name System — converts domain names (google.com) into IP addresses. 

Protocols: 

UDP 53 for queries. 

TCP 53 for zone transfers. 

Critical for nearly all networking & application communication. 

🖥️ RDP – Port 3389 

Purpose: Remote Desktop Protocol for Windows server login. 

Use Case: Managing Windows EC2 instances. 

Important: 

Restrict access — do not leave open to 0.0.0.0/0. 

📨 SMTP – Port 25 

Purpose: Email sending protocol. 

Notes: 

AWS often blocks port 25 by default to prevent spam. 

Alternative: 587 (TLS) or 465 (SMTPS). 

🗄️ MySQL – Port 3306 

Purpose: Access to MySQL database servers. 

Notes: 

Should be locked down to specific application servers or internal subnets only. 

🗄️ PostgreSQL – Port 5432 

Purpose: Access to PostgreSQL database servers. 

Notes: 

Never expose publicly — allow only internal/private traffic. 

 
 

✔️ Summary Table 

Service 

Port 

Purpose 

Security Notes 

SSH 

22 

Secure remote login (Linux) 

Restrict to your IP 

FTP 

21 

File transfer 

Not secure 

SFTP 

22 

Secure file transfer (over SSH) 

Preferred secure method 

HTTP 

80 

Unsecured web traffic 

Use only when necessary 

HTTPS 

443 

Secure web traffic (TLS/SSL) 

Industry standard 

DNS 

53 

Domain → IP resolution 

UDP/TCP 

RDP 

3389 

Remote login (Windows) 

Restrict access 

SMTP 

25 

Email sending 

Often blocked by AWS 

MySQL 

3306 

Database access 

Internal only 

PostgreSQL 

5432 

Database access 

Internal only 

 
