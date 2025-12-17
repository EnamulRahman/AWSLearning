AWS Serverless — Key Notes 

What Is Serverless? 

A way of building applications without managing servers. 

You focus only on writing and deploying code. 

AWS handles all infrastructure, scaling, patching, and maintenance. 

 
 

How Serverless Works 

You deploy functions, not servers. 

In AWS, this is mainly done using Lambda functions. 

Functions run only when triggered by events (e.g., API calls, file uploads, queue messages). 

You are billed only when the code runs, not for idle time. 

 
 

Function-as-a-Service (FaaS) 

The original meaning of “serverless.” 

Run individual functions on demand. 

Lambda is the main example. 

 
 

Serverless Also Includes Managed Services 

Any service where AWS hides the servers from you: 

DynamoDB (NoSQL database) 

S3 (storage) 

SQS (message queues) 

SNS, API Gateway, etc. 

These services scale automatically and require no server management. 

 
 

Important Clarification 

Serverless does NOT mean no servers exist. 

Servers still run your code/data — you just don’t have to manage them. 

AWS takes care of: 

Provisioning 

Scaling 

High availability 

Patching 

 
 

Why It Matters 

Lets you focus entirely on application logic. 

Reduces operational load and infrastructure complexity. 

Enables fast development and easy scaling. 

 

///////////////////////////////////////// 

 

Key AWS Serverless Services — Summary 

Core Compute 

AWS Lambda 

Write your code as functions; AWS runs them for you. 

No servers to manage. 

Event-driven and scales automatically. 

 
 

Databases & Storage 

DynamoDB 

Fully managed NoSQL database. 

Serverless, auto-scaling, no provisioning required. 

Amazon S3 

Serverless object storage. 

Stores files, static content, backups, logs. 

Scales automatically with no infrastructure management. 

Aurora Serverless 

Serverless relational database. 

Automatically scales capacity based on demand. 

 
 

User Authentication 

Cognito 

Handles sign-up, sign-in, and user authentication. 

Scales without managing servers. 

 
 

API Management 

API Gateway 

Creates and manages APIs. 

Acts as the bridge between users and backend Lambda functions or other services. 

 
 

Messaging & Event Services 

SNS (Simple Notification Service) 

Pub/sub messaging and notifications. 

SQS (Simple Queue Service) 

Message queue for decoupling applications. 

Stores messages safely until processed. 

Kinesis Data Firehose 

Serverless data streaming for real-time analytics. 

No servers to manage; auto-scales. 

 
 

Workflows & Orchestration 

Step Functions 

Coordinates multiple Lambda functions/services into workflows. 

Provides visual monitoring and built-in error handling. 

 
 

Containers 

AWS Fargate 

Serverless compute for containers (works with ECS/EKS). 

No EC2 instances to manage. 

 
 

How These Services Work Together (Typical Architecture) 

Users authenticate via Cognito. 

Static website content served from S3. 

User calls API → API Gateway. 

API triggers Lambda, which runs backend logic. 

Lambda reads/writes data to DynamoDB. 

Optional messaging handled by SNS/SQS. 

 
 

Key Takeaway 

AWS provides a full suite of serverless services for: 

Compute 

Storage 

Databases 

Authentication 

APIs 

Messaging 

Orchestration 

Containers 

All scale automatically, and you never manage servers. 

 

/////////////////////////////////////////////////////// 

 

 Why AWS Lambda 

Amazon EC2 

Virtual servers in the cloud 

Limited by RAM and CPU 

Continuously running 

Scaling requires manual intervention to add/remove servers 

 
 

Amazon Lambda 

Virtual functions — no servers to manage 

Limited by time — short executions 

Run on-demand 

Scaling is automated 

 

//////////////////////////////// 

 

Benefits of AWS Lambda 

Easy Pricing: 

Pay per request and compute time 

Free tier of 1,000,000 Lambda requests and 400,000 GB-seconds of compute time 

Integrated with the whole AWS suite of services 

Integrated with many programming languages 

Easy monitoring through AWS CloudWatch 

Easy to get more resources per function (up to 10 GB of RAM) 

Increasing RAM also improves CPU and network performance 

 

 

///////////////////////// 

 

 

 

 
