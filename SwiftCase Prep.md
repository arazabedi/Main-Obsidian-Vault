#InterviewPreparation
# Possible Questions

###### What do you know about case and workflow management?

Overview:
- Two approaches to building processes
- Both effective at process improvement and have many similarities, but also distinct differences
- One may be more suitable for a particular situation than another
- Used for digital transformation

Advantages Overview:
- Workflow management - ideal for linear processes that you can define
- Case management - more effective for complex, convoluted processes which often deviate from the norm

Workflow Automation
- Workflow management automates pieces of work that are predictable and follow a specific order
- Like a bus, which has a pre-determined route and specified stops, allowing people to get on off along the way
- In the context of accounts payable, it could work like this:

>[!example]
>1. Invoice comes in through email
>2. You upload to an OCR solution to extract information
>3. The document is routed to stakeholders for approvals
>4. Workflow software will post the payment to your accounting software

Automated Case Management:
- More flexible, provides tools giving you information to drive quick, informed decisions
- Allows you to stay on top of your tasks, checklists, and processes, but with flexibility to navigate casework differently each time
- An example would be HR software that contains all the information needed to bring an employee on board and sends documents for signing etc. plus has access to benefits and payroll.



###### What is your experience with AWS?

I'm aware of the importance of AWS. It started off as three main things: storage buckets, compute instances, and a messaging queue. It now has more than 200 services that all do similar things. Each service will cater to a specific use-case.

Interesting Examples:
1. RoboMaker - simulate and test robots at scale 
2. Braket - run code on quantum computers

Original Web Dev Services:
- Elastic Compute Cloud (EC2) - compute solution allowing you to create a virtual computer in the cloud (choose OS, memory, and computing power), then rent by paying for each second. Used as server for web applications.
- Cloud Watch - logs and metrics from each instance
- Auto Scaling - Creates new instances based on rules you define (you can send data from cloud watch)
- Load Balancing - distribute traffic to multiple instances automatically (if EC2 instance gets clogged)

New Web Dev Services:
- Elastic Beanstalk - All the above all in one service (Platform as a Service/PaaS)
- Lightsail - simpler alternative to Beanstalk with less customisation

Serverless (Functions as a Service):
- Lambda - Upload code then choose an event that decides when that code should run - benefit is you only pay the the exact number of requests and computing time that you use
- Serverless Application Repository - pre-built functions

Others:
- Outposts: Use AWS APIs on your own servers
- Elastic Container Registry: Store Docker images
- (ECS) Elastic Container Service: Run Docker images, stop, start, allocate virtual machines to virtual containers (e.g. connect to load balancer)
- EKS (Elastic Kubernetes Service)
- Fargate - containers behave like serverless
- App Runner - easiest way to run docker images

File Storage:
- (S3) Simple Storage Service - stores any file (based on amazon ecommerce)
- Glacier - higher latency but lower cost
- Elastic Block Storage - fast and can handle lots of throughput (for intensive data processing) - requires more manual configuration
- Elastic File System - highly performant and fully managed - all the bells and whistles at a much higher cost

Database:
- SimpleDB - original service (NoSQL)
- DynamoDB - document db able to scale horizontally, fast and cheap, but no joins (bad at modelling relational data) and limited queries
- DocumentDB - Exactly the same as MongoDB
- RDS (Relational Database Service) - supports a variety of SQL options, and manages backups, patching, and scale
- Aurora - proprietary SQL (compatible with Postgres and MySQL) 5x faster than MySQL
- Aurora Serverless - easier to scale, and only pay when the database is in use
- Neptune - graph database best for highly-connected datasets (e.g. social graphs, recommendation engines)
- Elastic Cache - fully managed Redis (in-memory database extremely fast
- TImestream - time-series with built in function and features for analytics

Analytics
- Redshift - store analytics data (shift away from Oracle)
- Lake Formation - Raw data and files (unstructured)
- Kineses - Real-time data

Loads more...



###### What do you know about Swiftcase?
