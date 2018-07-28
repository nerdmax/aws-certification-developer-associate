# Learning note

- [Learning note](#learning-note)
  - [IAM (Identity and Access Management)](#iam-identity-and-access-management)
    - [User, Group, Policies](#user-group-policies)
    - [MFA (multi-factor authentication)](#mfa-multi-factor-authentication)
    - [Add users](#add-users)
    - [Apply an IAM password policy](#apply-an-iam-password-policy)
    - [Roles](#roles)
  - [EC2 (Amazon Elastic Compute Cloud)](#ec2-amazon-elastic-compute-cloud)
    - [EC2 features](#ec2-features)
    - [EC2 types](#ec2-types)
    - [EBS (Elastic Block Storage)](#ebs-elastic-block-storage)
      - [EBS Volume Types](#ebs-volume-types)
    - [Elastic Load Balancers](#elastic-load-balancers)
    - [Route 53](#route-53)
    - [CLI](#cli)
    - [EC2 with S3 Role Lab](#ec2-with-s3-role-lab)
    - [AWS Database Types](#aws-database-types)
    - [RDS - BACK UPS, MULTI - AZ & READ REPLICAS](#rds---back-ups-multi---az--read-replicas)
    - [Elasticache 101](#elasticache-101)
  - [S3](#s3)
    - [Brief introduction](#brief-introduction)
    - [S3 Security](#s3-security)
    - [S3 Encryption](#s3-encryption)
    - [CORS](#cors)
    - [Cloud Front (CDN content delivery network)](#cloud-front-cdn-content-delivery-network)
    - [S3 Performance Optionization](#s3-performance-optionization)
  - [Serverless](#serverless)
    - [Lambda](#lambda)
    - [API Gateway](#api-gateway)
    - [Lambda version control](#lambda-version-control)
    - [Step Functions](#step-functions)
    - [X - Ray](#x---ray)
    - [Advanced API Gateway](#advanced-api-gateway)
  - [DynamoDB](#dynamodb)
    - [DynamoDB Indexes](#dynamodb-indexes)
    - [Scan Vs Query](#scan-vs-query)
    - [DynamoDB Provisioned Throughput](#dynamodb-provisioned-throughput)
    - [DynamoDB Accellorator (DAX)](#dynamodb-accellorator-dax)
    - [Elasticache](#elasticache)
  - [AWS KMS](#aws-kms)
    - [AWS Key Management Service](#aws-key-management-service)
    - [KMS API Calls](#kms-api-calls)
    - [Envelope Encryption](#envelope-encryption)
    - [SQS](#sqs)
    - [SNS](#sns)
    - [SNS VS SES](#sns-vs-ses)
    - [Elastic Beanstalk](#elastic-beanstalk)
    - [Updating Elastic Beanstalk](#updating-elastic-beanstalk)
    - [Advanced Elastic Beanstalk](#advanced-elastic-beanstalk)
    - [RDS & Elastic Beanstalk](#rds--elastic-beanstalk)
    - [Kinesis](#kinesis)
  - [Developer Theory](#developer-theory)
    - [CI/CD](#cicd)
    - [AWS CodeCommit](#aws-codecommit)
    - [AWS CodeDeploy](#aws-codedeploy)
    - [AWS CodePipeline](#aws-codepipeline)
    - [CodeDeploy Advanced Settings](#codedeploy-advanced-settings)
  - [Advanced IAM](#advanced-iam)
    - [Web Identity Federation](#web-identity-federation)
    - [Cognito User Pools](#cognito-user-pools)
    - [Inline Policies vs. Managed Policies vs. Custom Policies](#inline-policies-vs-managed-policies-vs-custom-policies)
  - [SWF](#swf)
  - [VPC (Virtual Private Cloud)](#vpc-virtual-private-cloud)

## IAM (Identity and Access Management)

IAM has the following:

- Users
- Groups (A way to group our users and apply polices to them collectively)
- Roles
- Policy Documents

### User, Group, Policies

1.  Create groups
2.  Assign users to different groups
3.  Assign policies to groups

### MFA (multi-factor authentication)

Virtual MFA Applications will let you scan QR code and it will generate one 6 digits code and another 6 digits code after a few seconds.

### Add users

1.  Programmatic login with Access key ID & Secret access key (Which will not be seen later)
2.  Console login with User name & Password

### Apply an IAM password policy

We can set up password policy here.

### Roles

IAM roles are a secure way to grant permissions to entities that you trust .

## EC2 (Amazon Elastic Compute Cloud)

Virtual server

### EC2 features

- On Demand
  - Fix rate by the hour
- Reserved Instances
  - 1 year or 3 years terms
- Spot Instances
  - Only pay what you bid on.
  - Will not be charged if instance is terminated by Amazon EC2, but will be charged if user terminates the instance
- Dedicated Hosts
  - Physical EC2 server, for server-bound software licenses.

### EC2 types

- F for FPGA
- I for IOPS
- G - Graphics
- H - High Disk Throughput
- T cheap general purpose (think T2 Micro)
- D for Density
- R for RAM
- M - main choice for general purpose apps
- C for Compute
- P - Graphics (think Pics)
- X - Extreme Memory

### EBS (Elastic Block Storage)

Virtual disk.

#### EBS Volume Types

- SSD
  - General Purpose SSD (GP2)
    - General purpose, balances both price and performance.
    - Ratio of 3 IOPS per GB with up to 10,000 IOPS and the ability to burst up to 3000 IOPS for extended periods of time for volumes at 3334 GiB and above.
  - Provisioned IOPS SSD (I01)
    - Designed for I/O intensive applications such as large relational or NoSql databases.
    - Use if you need more than 10,000
- Magnetic
  - Throughput Optimized HDD (ST1)
    - Low cost HDD volume designed for frequently accessed, throughtput-intensive workloads
    - Big data
    - Data warehouses
    - Log processing
    - Cannot be a boot volume
  - Cold HDD (SC1)
    - Lowest Cost Storage for infrequently accessed workloads
    - File Server
    - Cannot be a boot volume.
  - Magnetic (Standard)
    - Previous Generation. Can be a boot volume.

### Elastic Load Balancers

- 3 Types of Load Balancers
  - Application Load Balancers
  - Network Load Balancers
  - Classic Load Balancers
- 504 Error means the gateway has timed out. This means that the application not responding within the idle timeout period.
  - Trouble shoot the application. Is it the Web Server or Database Server?
- If you need the IPv4 address of your end user, look for the X-Forwarded-For header.

### Route 53

- Route 53 is Amazon's DNS service
- Allows you to map your domain names to
  - EC2 Instances
  - Load Balancers
  - S3 Buckets

### CLI

- Least Privilege - Always give your users the minimum amount of access required.
- Create Groups - Assign your users to groups. Your users will automatically inherit the permissions of the group. The groups permissions are assigned using policy documents.
- Secret Access Key - You will see this only once. If you do not save it, you can delete the Key Pair (Access Key ID and Secret Access Key) and regenerate it. You will need to run aws configure again.
- Do not use just one access key - Do not create just one access key and share that with all your developers. If someone leaves the company on bad terms, then you will needd to delete the key and create a new one and every developer would then need to update their keys. Instead create one key pair per developer.
- You can use the CLI on your PC - You can install the CLI on your Mac, Linux or Windows PC. I personally use S3 to store all my files up in the cloud.

### EC2 with S3 Role Lab

- Roles allow you to not use Access Key ID's and Secret Access Keys. Always use roles, not access keys.
- Roles are preferred from a security perspective
- Roles are controlled by policies
- You can change a policy on a role and it will take immediate affect
- You can attach and detach roles to running EC2 instances without having to stop or terminate these instances
- You can encrypt the root device volume (the volume the OS is installed on) using Operation System level encryption
- You can encrypt the root device volume by first taking a snapshot of that volume, and then creating a copy of that snap with encryption. You can then make an AMI of this snap and deploy the encrypted root device volume.
- You can encrypt additional attached volumes using the console, CLI or API.
- If you make an AMI publich, this AMI is immediately available across all regions, by default

### AWS Database Types

- RDS - OLTP (online transaction processing)
  - SQL
  - MySQL
  - PostgreSQL
  - Oracle
  - Aurora
  - MariaDB
- DynamoDB - No SQL
- RedShift - OLAP (On-line Analytical Processing)
  - Amazon's data warehousing service
- Elasticache - In Memory Caching
  - Memcached
  - Redis

In OLTP database there is detailed and current data, and schema used to store transactional databases is the entity model (usually 3NF). - OLAP (On-line Analytical Processing) is characterized by relatively low volume of transactions. Queries are often very complex and involve aggregations.

### RDS - BACK UPS, MULTI - AZ & READ REPLICAS

- Multi - AZ RDS
  - It's for disaster recovery only. It is not primarily used for improving performance. For performance improvement, you need Read Replicas.
- Read Replica Databases
  - Not for DR
  - Used for scaling, not for DR
  - Must have automatic backups turned on in order to deploy a read replica.
  - You can have up to 5 read replica copies of any database.
  - You can have read replicas of read replicas (but watch out for latency.)
  - Each read replica will have its own DNS end point.
  - You can have read replicas that have Multi - AZ.
  - You can create read replicas of Multi - AZ source databases.
  - Read replicas can be promoted to be their own databases. This breaks the replication.
  - You can have a read replica in a second region.

### Elasticache 101

- Memcached
  - If not concern about the redundancy
  - Object caching is your primary goal
  - You want to keep things as simple as possible
  - You want to scale your cache horizontally (Scale out)
- Redis
  - You have advanced data types, such as lists, hashes, and sets
  - You are doing data sorting and ranking (such as leader boards)
  - Data Persistence
  - Multi AZ
  - Pub/Sub capabilities are needed

Typically, you will be given a scenario where a particular database is under a lot of stress/load. You may be asked which service you should use to alleviate this.

Elasticache is a good choice if your database is particularly read-heavy and not prone to frequent changing.

Redshift is a good answer if the reason your database is feeling stress is because management keep running OLAP transactions on it etc.

## S3

Simple storage system

### Brief introduction

- Remember that S3 is Object-based: i.e. allows you to upload files. Object-based storage only (for files)
- Not suitable to install an operation system or running a database on
- Files can be from 0 Bytes to 5 TB.
- There is unlimited storage.
- Files are stored in Buckets.
- S3 is a unviersal namespace. That is, names must be unique globally.
- Read after Write consistency for PUTS of new Objects
- Eventual Consistency for overwrite PUTS and DELETES (can take some time to propagate)
- S3 Storage Classes/Tiers:
  - S3 (durable, immediately available, frequently accessed)
  - S3 - IA (durable, immediately available, infrequently accessed)
  - S3 - One Zone IA: Same as IA. However, data is stored in a single Availability Zone only.
  - S3 - Reduced Redundancy Storage (data that is easily reproducible, such as thumbnails, etc.)
  - Glacier - Archived data, where you can wait 3 - 5 hours before accessing
- Remember the core fundamentals of an S3 object:
  - Key (name)
  - Value (data)
  - Version ID
  - Metadata
  - Subresources - bucket-specific configuration:
    - Bucket Policies, Access Control Lists,
    - Cross Origin Resource Sharing (CORS)
    - Transfer Acceleration
- Successful uploads will generate a HTTP 200 status code. - when you use the CLI or API

### S3 Security

### S3 Encryption

- Type 1 Encryption In-Transit
  - SSL/TLS(HTTPS)
- Type 2 Encryption At Rest
  - Server Side Encryption
    - SSE-S3 (Keys are managed in S3)
    - SSE-KMS (Key management service)
    - SSE-C (Custom manage keys)
  - Client Side Encryption
- If you want to enforce the use of encryption for your files stored in S3, use an S3 Bucket Policy to deny all PUT requests that don't include the x-amz-server-side-encryption parameter in the request header.
- Default encryption is Advanced Encryption Standard (AES) 256

### CORS

- Used to enable cross origin access for your AWS resources
- e.g. S3 hosted website accessing javascript or image files located in another S3 bucket
- By default resources in one bucket cannot access resources located in another
- To allow this we need to configure CORS on the bucket being accessed and enable access for the origin (bucket) attempting to access
- Always use the S3 webiste URL, not the regular bucket URL:
  - website URL: (http://acloudguru.s3-website.eu-west-1.amazonaws.com)
  - regular bucket URL: https://s3-eu-west-1.amazonaws.com/acloudguru

### Cloud Front (CDN content delivery network)

- Edge Location - This is the location where content will be cached. This is separate to an AWS Region / AZ.
- Origin - This is the origin of all the files that the CDN will distribute. Origins can be an S3 Bucket, an EC2 Instance, an Elastic Load Balancer, or Route53.
- Distribution - This is the name given the CDN, which consists of a collection of Edge Locations.
  - Web Distribution - Used for Websites, HTTP/HTTPS
  - RTMP Distribution - (Adobe Real Time Messaging Protocol) Used for Media Streaming
- Edge locations are not just READ only - you can WRITE to them, too (i.e. PUT an obect on to them.)
- CloudFront Edge Locations are utilised by S3 Transfer Acceleration to reduce latency for S3 uploads.
- Objects are cached for the life of the TTL (Time To Live.)
- You can clear cached objects, but you will be charged.

### S3 Performance Optionization

- Remember the 2 main approaches to Performance Optiomization for S3:
  - GET - Intensive Workloads - Use CloudFront
  - Mixed - Workloads - Avoid sequential key names for your D3 objects. Instead, add a random prefix like a hex hash to the key name to prevent multiple objects from being stored on the same partition.

## Serverless

### Lambda

- Lambda scales out (not up) automatically
- Lambda functions are independent, 1 envent = 1 function
- Lambda is serverless
- Know what services are serverless!
  - Lambda is compute service
  - Lambda, API gateway, S3, DynamoDB, etc are serverless
  - RDB is not serverless
- Lambda functions can trigger other lambda functions, 1 enent can = x functions if functions trigger other functions
- Architectures can get extremely complicated, AWS X-ray allows you to debug what is happening.
- Lambda can do things globally, you can use it to back up S3 buckets to other S3 buckets etc
- Know your triggers

### API Gateway

- Remember what API Gateway is at a high level
- API Gateway has caching capabilities to increase performance
- API Gateway is low cost and scales automatically
- You can throttle API Gateway to prevent attacks
- You can log results to CloudWatch
- If you are using Javascript/AJAX that uses multiple domains with API Gateway, ensure that you have enabled CORS on API Gateway
- CORS is enforced by the client

### Lambda version control

- Can have multiple versions of lambda functions
- Latest version will use $latest
- Qualified version will use $latest, unqualified will not have it
- Versions are immutable (Cannot be changed).
- Can split traffic using aliases to different versions
  - Cannot split traffic with $latest, instead create an alias to latest

### Step Functions

- Great way to visualize your serverless application.
- Step Functions automatically triggers and tracks each step.
- Step Functions logs the state of each step so if something goes wrong you can track what went wrong and where.

### X - Ray

- The X-Ray SDK provides:
  - Interceptors to add to your code to trace incoming HTTP requests
  - Client handlers to instrument AWS SDK clients that your application uses to call other AWS services
  - An HTTP client to use to instrument calls to other internal and external HTTP web services
- The X-Ray Integrates with the following AWS services:
  - Elastic Load Balancing
  - AWS Lambda
  - Amazon API Gateway
  - Amazon Elastic Compute Cloud
  - AWS Elastic Beanstalk
- The X-Ray Integrates with the following languages:
  - Java
  - Go
  - Node.js
  - Python
  - Ruby
  - .Net

### Advanced API Gateway

- Import API's using Swagger 2.0 definition files
- API Gateway can be throttled
  - Default limits area 10,000 RPS or 5000 concurrently
- You can configure API Gateway as a SOAP Webservice passthrough

## DynamoDB

- Amazon DynamoDB is a low latency NoSQL database
- Consists of Tables Items and Attributes
- Supports both document and key-value data models
- Supported document formats are JSON, HTML, XML
- 2 types of Primary Key - Partition Key (User id / product id) and combination of Partition Key + Sort Key (Time stamp / date) (Composite Key)
- 2 Consistency models: Strongly Consistent (Most up to date data) / Eventually consistent (The data that is read may not be the latest one)
- Access is controlled using IAM policies
- Fine grained access control using IAM Condition parameter. dynamodb:LeadingKeys to allow users to access only the items where the partition key value matches their user ID

### DynamoDB Indexes

- Indexes enable fast queries on specific data columns.
- Give you a different view of your data, based on alternative Partition / Sort Keys
- Important to understand the differences

| Local Secondary Index                         | Global Secondary Index                           |
| --------------------------------------------- | ------------------------------------------------ |
| Must be created at when you create your table | Can create any time - at table creation or after |
| Same Partition key as your table              | Different Partition Key                          |
| Different Sort Key                            | Different Sort Key                               |

### Scan Vs Query

- A Query operation finds items in a table using only the Primary Key attribute.
- You provide the Primary Key name and a distinct value to search for
- A Scan operation examines every item in the table.
  - By default returns all data attributes.
  - Use the ProjectionExpression parameter to refine the results.
- Query results are always sorted by the Sort Key is there is one.
  - Sorted in ascending order.
  - Set ScanIndexForward parameter to false to reverse the order - queries only.
- Query operation is generally more efficient than a Scan.
- Reduce the impact to a query or scan by setting a smaller page size which uses fewer read operations.
- Isolate scan operations to specific tables and segregate them from your mission-critical traffic.
- Try Parallel scans, rather than the default sequential scan.
- Avoid using scan operations if you can : design tables in a way that you can use the Query, Get, or BatchGetItem APIs.

### DynamoDB Provisioned Throughput

- Provisioned Throughput is measured in Capacity Units.
- 1 x Write Capacity Unit = 1 x 1KB Write per second.
- 1 x Read Capacity Unit = 1 x 4KB Strongly Consistent Read OR 2 x 4KB Eventually Consistent Reads per second.
- Calculate Write Capacity Requirements (100 x 512 byte items per second):
  - 512 bytes / 1KB = 0.5
  - Rounded-up to the nearest whole number 1
  - 1 x 100 = 100 Write Capacity Units required
- Calculate Read Capacity Requirements (80 x 3KB items per second):
  - 3KB / 1KB = 0.333333
  - Rounded-up to the nearest whole number 1
  - 1 x 80 = 80 Read Capacity Units required
  - 80 / 2 = 40 if Eventual Consistency is acceptable

### DynamoDB Accellorator (DAX)

- Provides in-memory caching for DynamoDB tables
- Improves response times tor Eventually Consistent reads only
- You point your API calls the DAX cluster, instead of your table.
- If the item you are querying is on the cache, DAX will return it; otherwise it will perform an Eventually Consistent GetItem operation to your DynamoDB table.
- Not suitable for write-intensive application or applications that require Strongly Consistent reads.

### Elasticache

- In-memory cache sits between your application and database
- 2 different caching strategies: Lazy loading and Write Through
- Lazy Loading only caches the data when it is requested
  - Elasticache Node failures not fatal, just lots of cache misses
  - Cache miss penalty: Initial request, query database, writing to cache
  - Avoid stale data by implementing a TTL
- Write Through strategy writes data into the cache whenever there is a change to the database
  - Data is never stale
  - Write penalty: Each write involves a write to the cache
  - Elasticache node failure means that data is missing until added or updated in the database
  - Wasted resources if most of the data is never used

## AWS KMS

### AWS Key Management Service

- The Customer Master Key:
  - CMK
    - alias
    - creation date
    - description
    - key state
    - key material (either customer provided or AWS provided).
  - Can NEVER be exported
- Setup a Customer Master Key:
  - Create Alias and Description
  - Choose material option...
  - Define Key Administrative Permissions
    - IAM users/roles that can administer (but not use) the key through the KMS API.
  - Define Key Usage Permissions
    - IAM users/roles that acn use the key to encrypt and decrypt data.
- Key material options:
  - Use KMS generated key material
  - Your own key material

### KMS API Calls

- aws kms encrypt
- aws kms decrypt
- aws kms re-encrypt
- aws kms enable-key-rotation

### Envelope Encryption

- The Customer Master Key:
  - Customer Master Key used to decrypt the data key (envelope key)
  - Envelope Key is used to decrypt the data

### SQS

- SQS is a distributed message queueing system
- Allows you to decouple the components of an application so that they are independent
- Pull-based, not push-based
- Standard Queues (default) - best-effort ordering; message delivered at least once
- FIFO Queues (First In First Out) - ordering strictly preserved, message delivered once, no duplicates. e.g. good for banking transactions which need to happen in strict order.
- Visibility Timeout
  - Default is 30 seconds - increase if your task takes > 30 seconds to complete
  - Max 12 hours
- Short Polling - returned immediately even if no messages are in the queue
- Long Polling - polls the queue periodically and only returns a response when a message is in the queue or the timeout is reached
  - Maximum time out is 20 seconds

### SNS

- SNS is a scalable and highly available notification service which allows you to send push notifications from the cloud
- Variety of message formats supported: SMS text message, email, Amazon Simple Queue Service (SQS) queues, any HTTP endpoint.
- Pub-sub model whereby users subscribe to topics
- It is a push mechanism, rather than a pull(poll) mechanism

### SNS VS SES

- Remember that SES is for email only
- It can be used for incoming and outgoing mail
- It is not subscription based, you only need to know the email address
- SNS supports multiple formats (SMS, SQS, HTTP, email)
- You can fan-out messages to large number of recipients, (e.g. multiple clients each with their own SQS queue)

### Elastic Beanstalk

- Deploys and scales your web applications including the web application server platform where required
- Supports widely used programming technologies - Java, PHP, Python, Ruby, Go, Docker, .Net, Node.js
- And application server platforms like Tomcat, Passenger, Puma, and IIS
- Provisions the underlying resources for you
- Can fully manage the EC2 instances for you or you can take full administrative control
- Updates, monitoring, metrics and health checks all included

### Updating Elastic Beanstalk

- Remember the 4 different deployment approaches:
  - All at Once
    - Service interruption while you update the entire environment at once
    - To roll back, perform a further all at once upgrade
    - Only for test/dev
  - Rolling (Multiple EC2 instances)
    - Reduced capacity during deployment
    - To roll back, perform a further rolling update
  - Rolling with Additional Batch (Multiple EC2 instances)
    - Maintains full capacity
    - To roll back, perform a further rolling update
  - Immutable
    - Preferred option for mission critical production systems
    - Maintains full capacity
    - To roll back, just delete the new instances and autoscaling group
    - Best for production

### Advanced Elastic Beanstalk

- You can customize your Elastic Beanstalk environment by adding configuration files
- The files are written in YAML or JSON
- Files have a .config extension
- The .config files are saved to the .ebextensions folder
- Your .ebextensions folder must be located in the top level directory of your application source code bundle

### RDS & Elastic Beanstalk

- Two different options for launching your RDS instance:
- Launch within Elastic Beanstalk
  - When you terminate the Elastic Beanstalk environment, the database will also be terminated
  - Quick and easy to add your database and get started
  - Suitable for Dev and Test environments only
- Launch outside of Elastic Beanstalk
  - Additional configuration steps required - Security Group and Connection information
  - Suitable for Production environments, more flexibility
  - Allows connection from multiple environments, you can tear down the application stack without impacting the database

### Kinesis

- Know the difference between Kinesis Streams and Kinesis Firehose, You will be given scenario questions and you must choose the most relevant service.
  - Kinesis Streams
    - Video Streams - securely stream video from connected devices to AWS for analytics and machine learning
    - Data Streams - Build custom applications process data in real-time
  - Kinesis Firehose
    - capture, transform, load data streams into AWS data stores for near real-time analytics with BI tools
  - You can configure Lambda to subscribe to a Kinesis Stream and execute a function on your behalf when a new record is detected, before sending the processed data on to its final destination

## Developer Theory

### CI/CD

- Continuous Integration is about integrating or merging the code changes frequently - at least once per day, enables multiple devs to work on the same application
- Continuous Delivery is all about automating the build, test, and deployment functions.
- Continuous Deployment fully automates the entire release process, code is deployed into Production as soon as it has successfuly passed through the release pipeline
- AWS CodeCommit - Source Control service (git)
- AWS CodeBuild - compile source code, run tests and package code
- AWS CodeDeploy - Automated Deployment to EC2, on premises systems and Lambda
- AWS CodePipeline - CI/CD workflow tool, fully automates the entire release process (build, test, deployment)

### AWS CodeCommit

- AWS CodeCommit
  - Based on Git
  - Centralized repository for all your code, binaries, images, and libraries
  - Tracks and manages code changes
  - Maintains version history
  - Manages updates from multiple sources and enables collaboration

### AWS CodeDeploy

- AWS CodeDeploy is a fully managed automated deployment service and can be used as part of a Continuous Delivery or Continuous Deployment process.
- Remember the different types of deployment approach:
  - In-Place or Rolling update - you stop the application on each host and deploy the latest code. EC2 and on premise systems only. To roll back you must re-deploy the previous version of the application
  - Blue / Green - New instances are provisioned and the new application is deployed to these new instances. Traffic is routed to the new instances according to your won schedule. Supported for EC2, on-premise systems and Lambda functions. Roll back is easy, just route the traffic back to the original instances. Blue is the active deployment, green is the new release.

### AWS CodePipeline

- Continuous Integration / Continuous Delivery service
- Automates your end-to-end software release process based on a user defined workflow
- Can be configured to automatically trigger your pipeline as soon as a change is detected in your source code repository
- Integrates with other services from AWS like CodeBuild and CodeDeploy as well as third party and custom plugins

### CodeDeploy Advanced Settings

- The AppSpec file defines all the parameters needed for the deployment e.g. location of application files and pre/post deployment validation tests to run
- For EC2 / On Premises systems, the appspec.yml file must be placed in the root directory of your revision (the same folder taht contains your application code). Written in YAML
- Lambda supports YAML or JSON
- The run order for hooks in a CodeDeploy deployment:
  - BeforeBlockTraffic -> BlockTraffic -> AfterBlockTraffic
  - ApplicationStop
  - BeforeInstall
  - Install
  - AfterInstall
  - ApplicationStart
  - ValidateService
  - BeforeAllowTraffic -> AllowTraffic -> AfterAllowTraffic

## Advanced IAM

### Web Identity Federation

- Federation allows users to authenticate with a Web Identity Provider (Google, Facebook, Amazon)
- The user authenticates first with the Web ID Provider and receives an authentication token, which is wxchanged for temporary AWS credentials allowing them to assume an IAM role.
- Congnito is an Identity Broker which handles interaction between your applications and the web ID provider (You don't need to write your own code to do this.)
  - Provides sign-up, sign-in, and guest user access
  - Syncs user data for a seamless experiences across your devices
  - Cognito is the AWS recommended approach for web ID Federation particularly for mobile apps

### Cognito User Pools

- Cognito uses Users Pools to manage user sign-up and sign-in directly or via Web Identity Providers.
- Cognito acts as an Identity broker, handling all interaction with Web Identity Providers.
- Cognito uses Push Synchronization to send a silent push notification of user data updates to multiple device types associated with a user ID

### Inline Policies vs. Managed Policies vs. Custom Policies

- Remember the 3 different types of IAM Policies:
  - Managed Policy - AWS-managed default policies
  - Customer Managed Policy - Managed by you
  - Inline Policy - Managed by you and embedded in a single user, group, or role.
- In most cases, AWS recommends using Managed Policies over Inline Policies.

## SWF

- Domain
  - You define logical containers called domains for your application resources.
- Workers
  - Workers are programs that interact with Amazon SWF to get tasks, process received tasks, and return the results.
- Decider
  - The decider is a program that controls the coordination of tasks.

## VPC (Virtual Private Cloud)

- VPC lets you provision a logically isolated section of the AWS cloud where you can launch AWS resources in a virtual network that you define.
- You have complete control over your virtual networking environment, including selection of your own IP address range, creation of subnets, and configuration of route tables and network gateways.
- Security groups act like a firewall at the instance level whereas Network ACLs are an additional layer of security that act at the subnet level
- In Amazon VPC, an instance retains its private IP
- It is possible to have private subnet in VPC
- A subnet can not be associated with multiple Access Control Lists
- You may only have 1 internet gateway per VPC
- 5 VPCs are allowed in each AWS Region by default
- Only 1 internet gateway can attach to custom VPC
- When create new subnets within acustom VPC, by default they can communicate with each other, across availablilty
