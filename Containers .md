Docker on an OS 
Server (e.g., EC2 instance) 
NGINX 
Java 
MySQL 
NGINX 
Java 
Java 
1 
  Docker vs Virtual Machines — Key Notes 

Virtual Machines (VMs) 

Full virtual systems that simulate hardware. 

Each VM has its own guest OS (Ubuntu, Windows, etc.). 

Managed by a hypervisor. 

High resource usage — each VM needs: 

Its own OS 

Significant CPU, RAM, storage 

Slower to boot and heavier to run. 

Essentially like running multiple full computers on one host. 

 
 

Containers (Docker) 

Use containerization → run apps in isolated environments. 

Share the host OS kernel, no separate guest OS. 

Managed by the Docker daemon (lightweight vs hypervisor). 

Very lightweight: 

Lower CPU & memory usage 

No separate OS per container 

Fast to start/stop. 

Highly portable → run anywhere Docker is supported. 

Can run many containers on the same machine efficiently. 

 
 

Key Differences 

VMs virtualize hardware → heavy, slow, full OS per instance. 

Docker virtualizes the OS → lightweight, efficient, app-level isolation. 

VMs = multiple full OSes 

Containers = multiple apps sharing one OS kernel 

 
 

Why Containers Are Preferred 

Speed 

Portability 

Efficiency 

Great for microservices and modern deployments. 

 

 

How Docker Works — Quick Notes + Commands 

1. Create a Dockerfile 

A Dockerfile is the recipe/blueprint for building an image. 

Common Dockerfile instructions: 

FROM → base image 

COPY → copy app files into the image 

RUN → commands to install dependencies 

CMD → command executed when container starts 

Example Dockerfile 

FROM ubuntu:18.04 
COPY . /app 
RUN apt-get update && apt-get install -y python3 
CMD ["python3", "/app/app.py"] 
 

 
 

2. Build the Docker Image 

Turns your Dockerfile into an image. 

Command 

docker build -t myapp:latest . 
 

-t myapp:latest = tag the image 

. = build context (current folder) 

 
 

3. Run a Container 

Runs your image as a container. 

Command 

docker run --name mycontainer myapp:latest 
 

Optional common flags: 

docker run -d -p 8080:8080 --name mycontainer myapp:latest 
 

-d → run in background 

-p → map container port to host port 

 
 

4. View Images & Containers 

List images: 

docker images 
 

List running containers: 

docker ps 
 

List ALL containers: 

docker ps -a 
 

 
 

5. Push Image to a Registry (Docker Hub or ECR) 

Login (Docker Hub example): 

docker login 
 

Tag the image for your repo: 

docker tag myapp:latest username/myapp:latest 
 

Push: 

docker push username/myapp:latest 
 

 
 

6. Pull Image From Registry 

docker pull username/myapp:latest 
 

 
 

Summary 

Dockerfile = blueprint 

docker build = create image 

docker run = start container 

docker push/pull = store + retrieve images from a registry 

 

 

///////////////////// 

 

AWS Container Services — Key Notes 

Amazon ECS (Elastic Container Service) 

AWS’s managed container orchestration platform. 

Runs Docker containers without needing to install orchestration tools. 

You define: 

How many containers to run 

What images to use 

Networking + task configuration 

 
 

Amazon EKS (Elastic Kubernetes Service) 

AWS’s managed Kubernetes service. 

Runs Kubernetes without managing the control plane. 

Supports full Kubernetes features: 

Auto-scaling 

Load balancing 

Service discovery 

Best for teams already using Kubernetes. 

 
 

AWS Fargate 

Serverless container compute for ECS and EKS. 

You do not manage EC2 instances. 

You only define: 

Task size (CPU/RAM) 

Container image 

AWS handles infrastructure automatically. 

Great for no-maintenance, pay-per-use container workloads. 

 
 

Amazon ECR (Elastic Container Registry) 

AWS’s private container image repository. 

Similar to Docker Hub but integrated with ECS/EKS. 

Stores and manages Docker images. 

Private by default; can host public images too. 

 
 

Summary 

ECS → AWS-managed container orchestration (simple, non-Kubernetes). 

EKS → Managed Kubernetes for advanced K8s users. 

Fargate → Serverless containers (no servers/EC2 management). 

ECR → Docker image registry tightly integrated with AWS. 

 

 

////////////////////////////////////// 

 

 

ECS Launch Types — Key Notes 

1. EC2 Launch Type 

You manage the infrastructure (EC2 instances). 

Containers run on your EC2 instances. 

Each EC2 instance must run the ECS agent 
→ Registers the instance into the cluster. 

ECS handles the container lifecycle (start/stop), but you: 

Provision EC2 instances 

Patch/secure/scale them 

Handle capacity and maintenance 

Best when you need: 

Full control over instances 

Custom AMIs 

Compliance or performance tuning 

 
 

2. Fargate Launch Type (Serverless Containers) 

No EC2 instances required → fully managed by AWS. 

You only define the task definition: 

CPU 

Memory 

Network settings 

Container image 

Scaling = just increase number of tasks; AWS handles the rest. 

Ideal for: 

Teams wanting zero infrastructure management 

Flexible workloads 

Simplified scaling and operations 

 
 

Summary 

ECS EC2 Launch Type → You control EC2 infrastructure; ECS manages containers. 

ECS Fargate Launch Type → Fully serverless; AWS runs containers for you. 

 

///////// 

 

 

ECS Service Auto Scaling 

Automatically increase/decrease the desired number of ECS tasks. 

Amazon ECS Auto Scaling uses AWS Application Auto Scaling: 

ECS Service Average CPU Utilization 

ECS Service Average Memory Utilization – Scale on RAM 

ALB Request Count Per Target – metric coming from the ALB 

Target Tracking – Scale based on target value for a specific CloudWatch metric 

Step Scaling – Scale based on a specified CloudWatch Alarm 

Scheduled Scaling – Scale based on a specified date/time (predictable changes) 

ECS Service Auto Scaling (task level) = EC2 Auto Scaling (EC2 instance level) 

Fargate Auto Scaling is much easier to set up (because Serverless) 

 

 

///////////////////////////// 

 

Amazon ECR (Elastic Container Registry) — Key Notes 

What Is ECR? 

AWS’s cloud-based Docker image storage system. 

Works like a container image hub (similar to Docker Hub) but fully integrated with AWS. 

Used to store, manage, and pull Docker images. 

 
 

Repository Types 

Private Repositories 

For internal, secure, sensitive images. 

Public Repositories 

For open-source or publicly shared images (ECR Public Gallery). 

 
 

Integration with ECS 

ECS tasks can automatically pull images from ECR. 

ECR is backed by Amazon S3, ensuring durable and secure storage. 

 
 

Security 

Access controlled via IAM policies. 

If image pulls fail → check IAM permissions. 

 
 

Useful Features 

Vulnerability Scanning 

Scans images for security issues. 

Versioning (Tagging) 

Manage multiple versions of the same image. 

Lifecycle Policies 

Auto-delete old/unused images to save storage. 

 

///////////////////////////////////////// 

 

 

 Amazon Elastic Kubernetes Service — Key Notes 

What It Is 

A fully managed service that lets you run Kubernetes on Amazon Web Services. 

Amazon handles: 

Cluster control plane 

Updates and patching 

High availability 

Automatic scaling of the control plane 

You focus only on running your container applications. 

 
 

Why Kubernetes Matters 

Kubernetes is an open-source system for orchestrating containers. 

Automates: 

Deployments 

Scaling 

Restarts and health checks 

Load balancing 

Ideal for managing large, complex container environments. 

 
 

Amazon Elastic Kubernetes Service vs Amazon Elastic Container Service 

Amazon Elastic Container Service 

Built specifically for Amazon Web Services. 

Simple to use, tightly integrated with Amazon services. 

Not designed to be portable between cloud providers. 

Amazon Elastic Kubernetes Service 

Based on Kubernetes, which is cloud-agnostic. 

Workloads can be moved between clouds (Google, Azure, on-premises). 

Best for companies already using Kubernetes or wanting portability. 

 
 

Launch Types 

EC2 Launch Type 

You manage the worker machines that run your containers. 

Amazon manages the Kubernetes control plane. 

Fargate Launch Type 

Fully serverless. 

No need to manage any machines. 

You only specify the resources your containers need. 

 
 

Common Use Cases 

Migrating existing Kubernetes environments to Amazon Web Services. 

Running workloads across multiple cloud platforms. 

Deploying clusters across multiple regions for resilience. 

Large teams needing portability and loosely coupled microservices. 

 
 

Monitoring and Insights 

Amazon CloudWatch Container Insights provides: 

Metrics 

Logs 

Performance monitoring 

Troubleshooting tools 

 
 

Summary 

Amazon Elastic Kubernetes Service lets you run Kubernetes in Amazon Web Services without managing the control plane. 

Ideal for teams already familiar with Kubernetes or needing cloud flexibility. 

Supports both machine-managed and serverless launch types. 

 

////////////////////////////// 

 

 

Amazon Elastic Kubernetes Service — Worker Node Architecture (Key Points) 

1. Worker Nodes 

These are EC2 instances. 

Run your pods and containers. 

Form the compute layer of the Kubernetes cluster. 

 
 

2. Load Balancer 

An Amazon Elastic Load Balancer distributes incoming user traffic. 

Sends requests evenly to worker nodes running your application. 

 
 

3. NAT Gateway 

Allows worker nodes in private subnets to: 

Pull container images 

Download updates 

Does this without exposing nodes to the public internet. 

 
 

4. Private Subnets 

Worker nodes are placed in private networks. 

Increases security by preventing direct external access. 

 
 

5. Multi-Availability Zone Design 

Resources are spread across multiple Availability Zones. 

Ensures: 

High availability 

Resilience if one zone fails 

 
 

Summary 

This architecture is the standard foundation for running secure, scalable, and highly available Kubernetes workloads on Amazon Web Services. 
Amazon Web Services handles most of the operational complexity so you can focus on building and running applications. 

 

/////////////////////////////////////// 

 

Amazon EKS – Node Types 

Managed Node Groups 

Creates and manages nodes (EC2 instances) for you 

Nodes are part of an Auto Scaling Group managed by EKS 

Supports On-Demand or Spot instances 

 
 

Self-Managed Nodes 

Nodes created by you and registered to the EKS cluster, managed by an Auto Scaling Group 

You can use prebuilt AMI (Amazon EKS-optimized AMI) 

Supports On-Demand or Spot instances 

 
 

AWS Fargate 

No maintenance required; no nodes managed 

 
