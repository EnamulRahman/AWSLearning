EBS Volumes — Key Notes 

What is an EBS Volume? 

A virtual storage device for EC2 instances. 

Works like a network drive, not a physical disk. 

Lives in the cloud and can be attached/detached from EC2 instances. 

 
 

Main Characteristics 

Persistent storage 

Data survives instance stop or termination (unless set to delete on termination). 

AZ-bound 

Each EBS volume is tied to a specific Availability Zone. 

Cannot move a volume across AZs directly. 

Can snapshot a volume → create a new volume in another AZ. 

Can be detached and reattached to the same or a different EC2 instance (within the same AZ). 

 
 

Why Use EBS? 

Long-term data storage 

Ideal for databases, logs, applications that require persistent data. 

Reliability 

Data remains even if instance is stopped/unplugged. 

Free tier 

AWS provides 30 GB of free EBS storage per month (GP2/GP3 SSD or magnetic). 

 
 

Key Takeaways 

EBS acts like a virtual hard drive for EC2. 

Data is persistent and safe independent of instance lifecycle. 

AZ-locked, but snapshots allow cross-AZ copying. 

Perfect for databases, logs, and storing files long-term. 

 

 

 

 

Amazon Machine Image (AMI) — Key Notes 

What is an AMI? 

AMI = Amazon Machine Image 

A blueprint/template for launching EC2 instances. 

Pre-configured with: 

Operating system 

Software 

Configurations 

Monitoring tools 

Any custom settings you choose 

 
 

Why AMIs Are Important 

Provide customized EC2 setups. 

Save time — no need to configure software manually for every new instance. 

Enable consistent instance launches across environments. 

Faster boot and deployment times. 

 
 

AMI Types 

1. Public AMIs 

Provided by AWS for everyone to use. 

Examples: Amazon Linux, Ubuntu, Windows Server. 

2. Custom (Private) AMIs 

Created by you from a configured EC2 instance. 

Useful for cloning environments or repeatedly deploying the same setup. 

3. Marketplace AMIs 

Provided by third-party vendors via AWS Marketplace. 

Often come with pre-installed commercial software. 

Some are free, some are paid. 

 
 

How AMIs Work 

AMIs are region-specific. 

You can copy an AMI to another region if needed. 

Used as the starting point when launching EC2 instances. 

 
 

Why AMIs Are Useful 

Automate EC2 deployment. 

Ensure consistency across multiple instances. 

Enable quick scaling — launch many identical instances from one AMI. 

Reduce human error in configuration. 

Allow rapid environment replication (dev, test, prod). 

 
 

Key Takeaways 

AMIs are templates for EC2. 

They include OS + software + configuration. 

Can be public, custom, or from the marketplace. 

Region-specific but copyable across regions. 

Essential for automation, scaling, and consistency. 

 

Amazon EFS (Elastic File System) — Key Notes 

What is Amazon EFS? 

A managed network file system by AWS. 

Provides shared file storage that can be mounted by multiple EC2 instances at the same time. 

Works like a network drive that all instances can access. 

 
 

How EFS Works 

Designed for multi-AZ setups. 

Data is automatically replicated across Availability Zones. 

Provides high availability and durability. 

Fully managed — AWS handles scaling, redundancy, and maintenance. 

 
 

Key Advantages 

1. Shared Storage 

Multiple EC2 instances can mount the same file system. 

Perfect for applications needing shared data: 

CMS platforms 

Web servers 

Distributed workloads 

2. Automatic Scalability 

Storage size grows automatically as you add data. 

No need to provision or resize volumes manually. 

3. Multi-AZ Reliability 

Data redundantly stored across AZs → safer for critical applications. 

 
 

Why EFS Is Useful 

Allows EC2 instances to access a common file system using NFS. 

Great for: 

Shared application data 

Centralised content libraries 

Multi-instance architectures 

Works similarly to mounting an external file system in Linux (NFS-based). 

 
 

Drawbacks / Considerations 

More expensive — roughly 3× the cost of GP2 (standard SSD EBS). 

Should only be used when shared, scalable, multi-AZ storage is required. 

 
 

Key Takeaways 

EFS = scalable, highly available, shared file system. 

Mountable on many EC2 instances simultaneously. 

Automatically scales with your data. 

Multi-AZ by design, but costly, so use when necessary. 

 

 

 
