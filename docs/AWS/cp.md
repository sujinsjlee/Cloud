## Cloud Practitioner Essential
- Cloud Front
    - Deliver content to customers through a global network of edge locations.

- AWS Trusted Advisor checks
    - AWS Trusted Advisor helps AWS customers proactively address potential issues, optimize their resources, and adhere to AWS best practices. 
    - Examine your AWS environment to check for redundancy shortfalls and overused resources.
    - Optimize costs, improve performance, and address security gaps

- AWS Config
    - AWS Config is a service that enables you to assess, **audit**, and evaluate the configuration of your AWS resources.

- Amazon Relational DB service
    - AWS RDS (Aurora, MySQL, PostgreSQL, MariaDB, Oracle and SQL Server)

- Amazon Simple Queue Service 
    - Amazon SQS lets you send, store, and receive messages between software components at any volume, without losing messages or requiring other services to be available.

- AWS Batch 
    - AWS Batch lets developers, scientists, and engineers efficiently run hundreds of thousands of batch and ML computing jobs while optimizing compute resources, so you can focus on analyzing results and solving problems.

- AWS Internet gateway 
    - An internet gateway is a horizontally scaled, redundant, and highly available VPC component that allows communication between your VPC and the internet. It supports IPv4 and IPv6 traffic. It does not cause availability risks or bandwidth constraints on your network traffic.

- NAT Gateway
    - A NAT gateway is a Network Address Translation (NAT) service. You can use a NAT gateway so that instances in a private subnet can connect to services outside your VPC but external services cannot initiate a connection with those instances.

- Amazon Redshift
    - The best price-performance for cloud data warehousing
    - Amazon Redshift uses SQL to analyze structured and semi-structured data across data warehouses, operational databases, and data lakes, using AWS-designed hardware and machine learning to deliver the best price performance at any scale.

- AWS WAF
    - Protect your web applications from common exploits

- Amazon GuardDuty
    - GuardDuty combines machine learning (ML) and integrated threat intelligence from AWS and leading third parties to help protect your AWS accounts, workloads, and data.

- AWS Artifact 
    - It is your go-to, central resource for compliance-related information that matters to you. It provides on-demand access to security and compliance reports from AWS and ISVs who sell their products on AWS Marketplace.

- Amazon Macie
    - Discover and protect your sensitive data at scale
    - Amazon Macie is a data security service that uses machine learning (ML) and pattern matching to discover and help protect your sensitive data.

- AWS Shield
    - Protect DDos attack
    - Maximize application availability and responsiveness with managed DDoS protection

- AWS Pricing Calculator 
    - It lets you explore AWS services, and create an estimate for the cost of your use cases on AWS.
- AWS Simple Monthly Calculator
    - estimate of the cost of deploying new application on AWS
- AWS Cost explorer
    - AWS Cost Explorer has an easy-to-use interface that **lets you visualize, understand, and manage your AWS costs and usage over time.** Get started quickly by creating custom reports that analyze cost and usage data.
    - In summary, while the AWS Simple Monthly Calculator is used for estimating prospective costs before deploying resources, AWS Cost Explorer is used for analyzing and understanding past costs to optimize spending.

- AWS CloudHSM
    - Manage single-tenant **hardware** security modules (HSMs) on AWS
    - AWS CloudHSM helps you meet corporate, contractual, and regulatory compliance requirements for data security.

- Amazon Aurora
    - Amazon Aurora is a relational database management system (RDBMS) built for the cloud with full MySQL and PostgreSQL compatibility. Aurora gives you the performance and availability of commercial-grade databases at one-tenth the cost.

- Consolidated biling for AWS org
    - Combined usage : You can combine the usage across all accounts in the org to share the volume pricing discounts

- S3
    - You can use Amazon S3 to host a static website

- Snowball Edge
    - AWS Snowball Edge is a type of Snowball device with on-board storage and compute power for select AWS capabilities. Snowball Edge can do local processing and edge-computing workloads in addition to transferring data between your local environment and the AWS Cloud.

- Amazon Inspector
    - Amazon Inspector automatically discovers workloads, such as Amazon EC2 instances, containers, and Lambda functions, and scans them for software vulnerabilities and unintended network exposure.
    - Amazon Inspector is an automated vulnerability management service that continually scans AWS workloads for software vulnerabilities and unintended network exposure.

- AWS Storage Gateway is a set of hybrid cloud storage services that provide on-premises access to virtually unlimited cloud storage.

- AWS customer remains responsible for complying with applicable compliance laws and regulations

- Elastic Load Balancing (ELB) automatically distributes incoming application traffic across multiple targets and virtual appliances in one or more **Availability Zones (AZs)**.

- AWS Elastic Beanstalk 
    - It deploys web applications so that you can focus on your business.
    - Elastic Beanstalk is a service for deploying and scaling web applications and services. Upload your code and Elastic Beanstalk **automatically handles the deployment—from capacity provisioning, load balancing, and auto scaling to application health monitoring.**

- AWS Fargate
    - Serverless compute for containers
    - AWS Fargate is compatible with both Amazon Elastic Container Service (Amazon ECS) and Amazon Elastic Kubernetes Service (Amazon EKS). 

- AWS Service Catalog
    - Create, share, organize, and govern your curated IaC templates    


- AWS Glue
    - AWS Glue is a serverless data integration service that makes it easier to discover, prepare, move, and integrate data from multiple sources for analytics, machine learning (ML), and application development.

- Amazon Redshift
    - Amazon Redshift uses SQL to analyze structured and semi-structured data across data warehouses, operational databases, and data lakes, using AWS-designed hardware and machine learning to deliver the best price performance at any scale.

- Amazon Quantum Ledger Database
    - Amazon Quantum Ledger Database (Amazon QLDB) is a fully managed ledger database that provides a transparent, immutable, and cryptographically verifiable transaction log.

- AWS Organizations is a service provided by Amazon Web Services (AWS) that allows you to consolidate multiple AWS accounts into an organization that you create and centrally manage. AWS Organizations simplifies the billing and management of multiple AWS accounts by providing a hierarchical structure and centralized controls.

- AWS Outposts Family : AWS Outposts is a family of fully managed solutions delivering AWS infrastructure and services to virtually any on-premises or edge location for a truly consistent hybrid experience. Outposts solutions allow you to extend and run native AWS services on premises, and is available in a variety of form factors, from 1U and 2U Outposts servers to 42U Outposts racks, and multiple rack deployments.

- AWS Transit Gateway connects your Amazon Virtual Private Clouds (VPCs) and on-premises networks through a central hub.

- AWS Key Management Service
    - Create and control keys used to encrypt or digitally sign your data

- AWS CloudFormation
    - Speed up cloud provisioning with infrastructure as code
    - AWS CloudFormation lets you model, provision, and manage AWS and third-party resources by treating infrastructure as code.

## Cloud Practitioner Exam

- [Exam Prep](https://explore.skillbuilder.aws/learn/course/internal/view/elearning/14050/aws-certified-cloud-practitioner-official-practice-question-set-clf-c02-english)


<!--
When architecting cloud applications, which of the following is a key design principle?
: Implement elasticity

Which of the following is an advantage of using AWS
: There is no guessing on capacity needs

A company needs hybrid storage solution that allows on-prem applications to use AWS cloud storage in a seamless fashion
: AWS Storage Gateway
: AWS Storatge Gateway connects an on-prem software appliance with cloud-based storage to provide seamless integration

In AWS Shared Responsibility Model, which of these security-related tasks are customer's responsibility?
: Maintaining server-side encryption

As per operational excellence in the cloud, which of the following is a critical architectural concept when designing cloud application?
: Design for failure



(((4))) According to security best practices, how should an Amazon EC2 instance be given access to an Amazon S3 bucket?

A. Hard code an IAM user’s secret key and access key directly in the application, and upload the file.
B. Store the IAM user’s secret key and access key in a text file on the EC2 instance, read the keys, then upload the file.
C. Have the EC2 instance assume a role to obtain the privileges to upload the file.
D. Modify the S3 bucket policy so that any service can upload to it at any time.

C


(((6))) Which option is a perspective that includes foundational capabilities of the AWS Cloud Adoption Framework (AWS CAF)?

A. Sustainability
B. Performance efficiency
C. Governance
D. Reliability

C

(((8))) A company wants to run a NoSQL database on Amazon EC2 instances.
Which task is the responsibility of AWS in this scenario?

A. Update the guest operating system of the EC2 instances.
B. Maintain high availability at the database layer.
C. Patch the physical infrastructure that hosts the EC2 instances.
D. Configure the security group firewall.

C

AWS provides the underlying infrastructure, but ensuring high availability at the database layer, such as implementing replication, clustering, or failover mechanisms, is typically the responsibility of the user. So answer is C



(((9))) Which AWS services or tools can identify rightsizing opportunities for Amazon EC2 instances? (Choose two.)

A. AWS Cost Explorer
B. AWS Billing Conductor
C. Amazon CodeGuru
D. Amazon SageMaker
E. AWS Compute Optimizer

AE


(((12))) A company wants to manage deployed IT services and govern its infrastructure as code (IaC) templates.
Which AWS service will meet this requirement?

A. AWS Resource Explorer
B. AWS Service Catalog
C. AWS Organizations
D. AWS Systems Manager

B


(((13))) Which AWS service or tool helps users visualize, understand, and manage spending and usage over time?

A. AWS Organizations
B. AWS Pricing Calculator
C. AWS Cost Explorer
D. AWS Service Catalog

C

(((14))) A company is using a central data platform to manage multiple types of data for its customers. The company wants to use AWS services to discover, transform, and visualize the data.
Which combination of AWS services should the company use to meet these requirements? (Choose two.)

A. AWS Glue
B. Amazon Elastic File System (Amazon EFS)
C. Amazon Redshift
D. Amazon QuickSight
E. Amazon Quantum Ledger Database (Amazon QLDB)

AD


(((16))) An e-learning platform needs to run an application for 2 months each year. The application will be deployed on Amazon EC2 instances. Any application downtime during those 2 months must be avoided.
Which EC2 purchasing option will meet these requirements MOST cost-effectively?

A. Reserved Instances
B. Dedicated Hosts
C. Spot Instances
D. On-Demand Instances

D

A. Reserved Instances (RIs): Reserved Instances provide a significant discount (compared to On-Demand pricing) in exchange for a commitment to a one- or three-year term.

B. Dedicated Hosts: Dedicated Hosts allow you to have dedicated physical servers for your EC2 instances. 

C. Spot Instances: Spot Instances can be terminated by AWS with little notice if the capacity is needed elsewhere, leading to potential downtime.

D. On-Demand Instances: On-Demand Instances are charged by the hour or second, with no upfront commitments. They are suitable for variable workloads, and you only pay for the compute capacity you use. For a workload that runs for a short duration each year without tolerance for downtime, On-Demand Instances are likely the most cost-effective and flexible option.

(((17))) A developer wants to deploy an application quickly on AWS without manually creating the required resources.
Which AWS service will meet these requirements?

A. Amazon EC2
B. AWS Elastic Beanstalk
C. AWS CodeBuild
D. Amazon Personalize

B

A. Amazon EC2: Amazon EC2 provides virtual servers in the cloud, but it requires manual configuration and management.

(((18))) A company is storing sensitive customer data in an Amazon S3 bucket. The company wants to protect the data from accidental deletion or overwriting.
Which S3 feature should the company use to meet these requirements?

A. S3 Lifecycle rules
B. S3 Versioning
C. S3 bucket policies
D. S3 server-side encryption

B

B. S3 Versioning: Amazon S3 versioning is a feature that helps maintain multiple versions of an object in a bucket. With versioning enabled, each time an object is overwritten or deleted, a new version of the object is created. This allows you to revert to a previous version in case of accidental deletion or modification. It provides an additional layer of data protection and helps prevent data loss.

C. S3 bucket policies: S3 bucket policies are used to control access to S3 buckets and objects. While they are essential for controlling permissions, they do not provide versioning capabilities for protecting against accidental deletion or overwriting.


(((19))) Which AWS service provides the ability to manage infrastructure as code?

A. AWS CodePipeline
B. AWS CodeDeploy
C. AWS Direct Connect
D. AWS CloudFormation

D

both AWS CloudFormation and AWS Service Catalog are part of AWS's Infrastructure as Code offerings, CloudFormation is more about defining and managing infrastructure, whereas Service Catalog is about governing and providing a self-service catalog of approved services. 

(((22))) Which option is a physical location of the AWS global infrastructure?

A. AWS DataSync
B. AWS Region
C. Amazon Connect
D. AWS Organizations

B

(((26))) A company has an AWS account. The company wants to audit its password and access key rotation details for compliance purposes.
Which AWS service or tool will meet this requirement?

A. IAM Access Analyzer
B. AWS Artifact
C. IAM credential report
D. AWS Audit Manager

C

B. AWS Artifact: AWS Artifact is a portal that provides access to compliance reports, including those related to GDPR, PCI DSS, and more. However, it is more focused on providing documentation and reports for compliance rather than real-time auditing of password and access key rotation.

To audit password and access key rotation details for compliance purposes in an AWS account, the appropriate tool to use is the IAM credential report.

C. IAM credential report: IAM (Identity and Access Management) credential reports provide information about AWS users and their associated security credentials. These reports include details such as when passwords were last used or changed, when access keys were last rotated, and other important security-related information. This information is valuable for auditing and compliance purposes.

(((27))) A company wants to receive a notification when a specific AWS cost threshold is reached.
Which AWS services or tools can the company use to meet this requirement? (Choose two.)

A. Amazon Simple Queue Service (Amazon SQS)
B. AWS Budgets
C. Cost Explorer
D. Amazon CloudWatch
E. AWS Cost and Usage Report

BD

C. Cost Explorer: AWS Cost Explorer is a tool for visualizing, understanding, and managing AWS costs and usage. It provides historical data and forecasting but does not offer direct notifications for cost threshold breaches.

E. AWS Cost and Usage Report: This report provides comprehensive cost and usage data at a detailed level. However, it is more about reporting and analysis than setting up real-time notifications for cost thresholds.

B. AWS Budgets: AWS Budgets is a service that allows you to set custom cost and usage budgets that alert you when you exceed your thresholds. You can configure AWS Budgets to send notifications via Amazon SNS (Simple Notification Service) when your costs or usage exceed the defined limits.

D. Amazon CloudWatch: Amazon CloudWatch allows you to create custom alarms based on various metrics, including AWS costs. You can set up a CloudWatch alarm to trigger when your costs reach a specified threshold. CloudWatch alarms can send notifications through Amazon SNS.

(((29))) Which AWS service or resource provides answers to the most frequently asked security-related questions that AWS receives from its users?

A. AWS Artifact
B. Amazon Connect
C. AWS Chatbot
D. AWS Knowledge Center

D

C. AWS Chatbot: AWS Chatbot is a service that enables interaction with AWS services through chat interfaces like Slack or Amazon Chime. While it can be used for operational tasks, it is not the primary resource for security-related FAQs.

D. AWS Knowledge Center is the service that provides answers to the most frequently asked security-related questions that AWS receives from its users.

(((29))) Which tasks are customer responsibilities, according to the AWS shared responsibility model? (Choose two.)

A. Configure the AWS provided security group firewall.
B. Classify company assets in the AWS Cloud.
C. Determine which Availability Zones to use for Amazon S3 buckets.
D. Patch or upgrade Amazon DynamoDB.
E. Select Amazon EC2 instances to run AWS Lambda on.

AB

Customer responsibility “Security in the Cloud” – Customer responsibility will be determined by the AWS Cloud services that a customer selects. This determines the amount of configuration work the customer must perform as part of their security responsibilities. For example, a service such as Amazon Elastic Compute Cloud (Amazon EC2) is categorized as Infrastructure as a Service (IaaS) and, as such, requires the customer to perform all of the necessary security configuration and management tasks. Customers that deploy an Amazon EC2 instance are responsible for management of the guest operating system (including updates and security patches), any application software or utilities installed by the customer on the instances, and the configuration of the AWS-provided firewall (called a security group) on each instance. For abstracted services, such as Amazon S3 and Amazon DynamoDB, AWS operates the infrastructure layer, the operating system, and platforms, and customers access the endpoints to store and retrieve data. Customers are responsible for managing their data (including encryption options), classifying their assets, and using IAM tools to apply the appropriate permissions.


(((30))) Which of the following are pillars of the AWS Well-Architected Framework? (Choose two.)

A. Availability
B. Reliability
C. Scalability
D. Responsive design
E. Operational excellence

The Correct answer is BE.

AWS Well-Architected helps cloud architects build secure, high-performing, resilient, and efficient infrastructure for a variety of applications and workloads. Built around six pillars:
operational excellence, 
security,
reliability, 
performance efficiency, 
cost optimization,  
sustainability.

(((31))) Which AWS service or feature is used to send both text and email messages from distributed applications?

A. Amazon Simple Notification Service (Amazon SNS)
B. Amazon Simple Email Service (Amazon SES)
C. Amazon CloudWatch alerts
D. Amazon Simple Queue Service (Amazon SQS)

A

(((32))) A user needs programmatic access to AWS resources through the AWS CLI or the AWS API.
Which option will provide the user with the appropriate access?

A. Amazon Inspector
B. Access keys
C. SSH public keys
D. AWS Key Management Service (AWS KMS) keys

B

(((35))) A company needs to block SQL injection attacks.
Which AWS service or feature can meet this requirement?

A. AWS WAF
B. AWS Shield
C. Network ACLs
D. Security groups

A. AWS WAF (Web Application Firewall): AWS WAF is a web application firewall that helps protect web applications from common web exploits, including SQL injection attacks. It allows you to create rules that block or allow web traffic based on conditions you define.

The other options:

B. AWS Shield: AWS Shield is a managed Distributed Denial of Service (DDoS) protection service. 

C. Network ACLs (Access Control Lists): Network ACLs are used to control traffic at the subnet level in a Virtual Private Cloud (VPC). 

D. Security groups: Security groups are used to control inbound and outbound traffic at the instance level. 


(((41))) Which service enables customers to audit API calls in their AWS accounts?

A. AWS CloudTrail
B. AWS Trusted Advisor
C. Amazon Inspector
D. AWS X-Ray

A

The service that enables customers to audit API calls in their AWS accounts is:

A. AWS CloudTrail: AWS CloudTrail is a service that logs all API calls made on your AWS account. It provides a record of actions taken by users, roles, or AWS services, and it includes information such as the identity of the caller, the time of the call, the source IP address, and more. CloudTrail logs are useful for security analysis, resource change tracking, and compliance auditing.

The other options:

B. AWS Trusted Advisor: AWS Trusted Advisor provides best practices and recommendations for improving your AWS environment. While it can offer guidance on security best practices, it does not provide detailed logs of API calls.

C. Amazon Inspector: Amazon Inspector is a security assessment service that helps improve the security and compliance of applications deployed on AWS. It focuses on assessing the security vulnerabilities of your EC2 instances rather than auditing API calls.

D. AWS X-Ray: AWS X-Ray is a service for tracing requests made to your application and provides insights into the application's behavior. It is used for monitoring and troubleshooting application performance rather than auditing API calls at the account level.

(((43))) A company has 5 TB of data stored in Amazon S3. The company plans to occasionally run queries on the data for analysis.
Which AWS service should the company use to run these queries in the MOST cost-effective manner?

A. Amazon Redshift
B. Amazon Athena
C. Amazon Kinesis
D. Amazon RDS

For the occasional analysis of data stored in Amazon S3, the most cost-effective option is:

B. Amazon Athena: Amazon Athena is a serverless query service that allows you to analyze data directly in Amazon S3 using standard SQL queries. It is a pay-per-query service, which means you only pay for the queries you run. Since the company plans to occasionally run queries, this on-demand pricing model can be more cost-effective compared to maintaining and managing a dedicated cluster (as in the case of Amazon Redshift).

The other options:

A. Amazon Redshift: Amazon Redshift is a fully managed data warehouse service that is designed for high-performance analysis using SQL queries. While powerful, Redshift involves provisioning and managing a cluster, and it may have higher ongoing costs, making it less suitable for occasional analysis.

C. Amazon Kinesis: Amazon Kinesis is a platform for collecting, processing, and analyzing streaming data. It is designed for real-time processing and analytics on streaming data, which might be overkill for occasional analysis of a 5 TB dataset.

D. Amazon RDS: Amazon RDS (Relational Database Service) is a managed relational database service. While it is suitable for storing and querying structured data, it may not be the most cost-effective solution for ad-hoc analysis of large datasets stored in Amazon S3.


(((44))) Which AWS service can be used at no additional cost?

A. Amazon SageMaker
B. AWS Config
C. AWS Organizations
D. Amazon CloudWatch

C

C. AWS Organizations: AWS Organizations is a service that helps you consolidate multiple AWS accounts into an organization that you create and centrally manage. The service itself does not incur any additional charges; however, you will still be billed for the resources and services used within your AWS accounts.

The other options:

A. Amazon SageMaker: Amazon SageMaker is a fully managed service for building, training, and deploying machine learning models. While there are costs associated with the resources used (such as instances for training), the SageMaker service itself is not free.

B. AWS Config: AWS Config provides a detailed view of the configuration of AWS resources in your AWS account. While the service itself has associated costs, they are generally low, and you will also be billed for the configuration items recorded.

D. Amazon CloudWatch: While Amazon CloudWatch itself has associated costs for certain features (e.g., custom metrics and alarms), the basic monitoring features, such as metrics from AWS services, are available at no additional cost.

(((45))) Which AWS Cloud Adoption Framework (AWS CAF) capability belongs to the people perspective?

A. Data architecture
B. Event management
C. Cloud fluency
D. Strategic partnership

C

Data architecture => Platform
Event management => Operations
Cloud fluency => People
Strategic partnership => Business

(((46))) A company wants to make an upfront commitment for continued use of its production Amazon EC2 instances in exchange for a reduced overall cost.
Which pricing options meet these requirements with the LOWEST cost? (Choose two.)

A. Spot Instances
B. On-Demand Instances
C. Reserved Instances
D. Savings Plans
E. Dedicated Hosts

CD

(((47))) A company wants to migrate its on-premises relational databases to the AWS Cloud. The company wants to use infrastructure as close to its current geographical location as possible.
Which AWS service or resource should the company use to select its Amazon RDS deployment area?

A. Amazon Connect
B. AWS Wavelength
C. AWS Regions
D. AWS Direct Connect

C


(((51))) Which of the following is a software development framework that a company can use to define cloud resources as code and provision the resources through AWS CloudFormation?

A. AWS CLI
B. AWS Developer Center
C. AWS Cloud Development Kit (AWS CDK)
D. AWS CodeStar

C

C. AWS Cloud Development Kit (AWS CDK): AWS CDK is an open-source software development framework that enables developers to define cloud infrastructure using familiar programming languages, such as Java, TypeScript, Python, and others. It allows you to define cloud resources as code using object-oriented programming constructs and then deploy and provision those resources using AWS CloudFormation.

The other options:

A. AWS CLI (Command Line Interface): AWS CLI is a command-line tool that allows users to interact with AWS services using commands. While it can be used for various tasks, it is not specifically designed for defining cloud resources as code.

B. AWS Developer Center: There is no specific service or framework known as "AWS Developer Center" for defining cloud resources as code. The AWS Developer Center generally provides resources, tools, and documentation for developers but is not focused on infrastructure as code.

D. AWS CodeStar: AWS CodeStar is a fully managed service that makes it easy to develop, build, and deploy applications on AWS. While it provides a set of development tools and templates, it is not primarily focused on defining cloud resources as code for use with AWS CloudFormation.

(((58))) Which AWS service gives users the ability to discover and protect sensitive data that is stored in Amazon S3 buckets?

A. Amazon Macie
B. Amazon Detective
C. Amazon GuardDuty
D. AWS IAM Access Analyzer

A

(((62))) Which AWS service supports a hybrid architecture that gives users the ability to extend AWS infrastructure, AWS services, APIs, and tools to data centers, co-location environments, or on-premises facilities?

A. AWS Snowmobile
B. AWS Local Zones
C. AWS Outposts
D. AWS Fargate

C

(((63))) Which AWS service can run a managed PostgreSQL database that provides online transaction processing (OLTP)?

A. Amazon DynamoDB
B. Amazon Athena
C. Amazon RDS
D. Amazon EMR


C. Amazon RDS (Relational Database Service): Amazon RDS provides a managed database service, including PostgreSQL as one of the available database engines. You can easily set up, operate, and scale a PostgreSQL database on Amazon RDS without managing the underlying infrastructure. It is suitable for OLTP workloads where you need a relational database with features like ACID compliance and support for transactions.

The other options:

A. Amazon DynamoDB: Amazon DynamoDB is a fully managed NoSQL database service that is designed for applications with high read and write throughput. It is not a relational database and is more suitable for use cases with high scalability requirements.

B. Amazon Athena: Amazon Athena is a serverless query service that enables you to analyze data stored in Amazon S3 using SQL queries. It is not a managed database service and is more focused on ad-hoc querying of data in S3.

D. Amazon EMR (Elastic MapReduce): Amazon EMR is a cloud-based big data platform that enables processing of large datasets using popular frameworks such as Apache Spark and Apache Hadoop. It is not a managed PostgreSQL database service and is typically used for big data processing rather than OLTP.

(((65))) A company wants to monitor for misconfigured security groups that are allowing unrestricted access to specific ports.
Which AWS service will meet this requirement?

A. AWS Trusted Advisor
B. Amazon CloudWatch
C. Amazon GuardDuty
D. AWS Health Dashboard

A


(((67))) A company is deploying a machine learning (ML) research project that will require a lot of compute power over several months. The ML processing jobs do not need to run at specific times.
Which Amazon EC2 instance purchasing option will meet these requirements at the lowest cost?

A. On-Demand Instances
B. Spot Instances
C. Reserved Instances
D. Dedicated Instances

B

(((68))) Which AWS services or features provide disaster recovery solutions for Amazon EC2 instances? (Choose two.)

A. EC2 Reserved Instances
B. EC2 Amazon Machine Images (AMIs)
C. Amazon Elastic Block Store (Amazon EBS) snapshots
D. AWS Shield
E. Amazon GuardDuty

BC

B. EC2 Amazon Machine Images (AMIs): EC2 AMIs allow you to create and maintain a snapshot of your EC2 instance's root volume and any additional attached volumes. This snapshot can be used to launch a new instance in the event of a failure or disaster. AMIs are a key component of backup and recovery strategies.

C. Amazon Elastic Block Store (Amazon EBS) snapshots: Amazon EBS snapshots are point-in-time copies of Amazon EBS volumes. They are often used as part of a disaster recovery strategy. Snapshots can be used to restore volumes or create new volumes in the event of data loss or corruption.

The other options:
D. AWS Shield: AWS Shield is a managed Distributed Denial of Service (DDoS) protection service. While it helps protect against DDoS attacks, it is not specifically a disaster recovery solution for EC2 instances.

E. Amazon GuardDuty: Amazon GuardDuty is a threat detection service that continuously monitors for malicious activity and unauthorized behavior in AWS accounts. While it enhances security, it is not specifically designed for disaster recovery.

(((69))) Which AWS service provides command line access to AWS tools and resources directly from a web browser?

A. AWS CloudHSM
B. AWS CloudShell
C. Amazon WorkSpaces
D. AWS Cloud Map

B

B. AWS CloudShell: AWS CloudShell is a browser-based shell that provides command-line access to AWS resources directly from the AWS Management Console. It comes pre-configured with AWS CLI (Command Line Interface) and other AWS tools, eliminating the need to install and configure them locally.

The other options:

A. AWS CloudHSM: AWS CloudHSM is a hardware security module (HSM) that provides secure key storage and cryptographic operations. It is not a service for command line access to AWS tools.

C. Amazon WorkSpaces: Amazon WorkSpaces is a fully managed, secure desktop computing service. It provides virtual desktops that users can access from various devices but is not specifically focused on command-line access.

D. AWS Cloud Map: AWS Cloud Map is a service for service discovery in cloud environments. It is not designed for providing command line access to AWS tools and resources.

(((71))) A company wants to assess its operational readiness. It also wants to identify and mitigate any operational risks ahead of a new product launch.
Which AWS Support plan offers guidance and support for this kind of event at no additional charge?

A. AWS Business Support
B. AWS Basic Support
C. AWS Developer Support
D. AWS Enterprise Support

D

(((73))) Which AWS service or feature can be used to create a private connection between an on-premises workload and an AWS Cloud workload?

A. Amazon Route 53
B. Amazon Macie
C. AWS Direct Connect
D. AWS PrivateLink

C

C. AWS Direct Connect: AWS Direct Connect is a network service that provides dedicated network connections from your on-premises data center to AWS. It allows you to establish private connectivity to AWS, bypassing the public internet. This private connection can be used to access resources in your Amazon Virtual Private Cloud (Amazon VPC) securely.

(((74))) Which AWS service is used to provide encryption for Amazon EBS?

A. AWS Certificate Manager
B. AWS Systems Manager
C. AWS KMS
D. AWS Config

C

(((82))) What are the benefits of consolidated billing for AWS Cloud services? (Choose two.)

A. Volume discounts
B. A minimal additional fee for use
C. One bill for multiple accounts
D. Installment payment options
E. Custom cost and usage budget creation

AC

(((83))) A user wants to review all Amazon S3 buckets with ACLs and S3 bucket policies in the S3 console.
Which AWS service or resource will meet this requirement?

A. S3 Multi-Region Access Points
B. S3 Storage Lens
C. AWS IAM Identity Center (AWS Single Sign-On)
D. Access Analyzer for S3

D

(((87))) Which AWS service provides highly durable object storage?

A. Amazon S3
B. Amazon Elastic File System (Amazon EFS)
C. Amazon Elastic Block Store (Amazon EBS)
D. Amazon FSx

A

(((89))) Which of the following are advantages of moving to the AWS Cloud? (Choose two.)

A. The ability to turn over the responsibility for all security to AWS.
B. The ability to use the pay-as-you-go model.
C. The ability to have full control over the physical infrastructure.
D. No longer having to guess what capacity will be required.
E. No longer worrying about users access controls.

BD

(((95))) Which AWS service or tool can be used to set up a firewall to control traffic going into and coming out of an Amazon VPC subnet?

A. Security group
B. AWS WAF
C. AWS Firewall Manager
D. Network ACL

A


(((98))) A company wants to grant users in one AWS account access to resources in another AWS account. The users do not currently have permission to access the resources.
Which AWS service will meet this requirement?

A. IAM group
B. IAM role
C. IAM tag
D. IAM Access Analyzer

B

(((106)))A developer has been hired by a large company and needs AWS credentials.
Which are security best practices that should be followed? (Choose two.)

A. Grant the developer access to only the AWS resources needed to perform the job.
B. Share the AWS account root user credentials with the developer.
C. Add the developer to the administrator’s group in AWS IAM.
D. Configure a password policy that ensures the developer’s password cannot be changed.
E. Ensure the account password policy requires a minimum length.

AE


(((112))) A company has an uninterruptible application that runs on Amazon EC2 instances. The application constantly processes a backlog of files in an Amazon Simple Queue Service (Amazon SQS) queue. This usage is expected to continue to grow for years.
What is the MOST cost-effective EC2 instance purchasing model to meet these requirements?

A. Spot Instances
B. On-Demand Instances
C. Savings Plans
D. Dedicated Hosts

C

C. Savings Plans

For a workload with expected long-term growth like the one described, Savings Plans would likely be the most cost-effective EC2 instance purchasing model. Savings Plans offer significant cost savings compared to On-Demand Instances in exchange for a commitment to a consistent amount (measured in $/hr) for a 1 or 3-year period.

(114) A company is planning its migration to the AWS Cloud. The company is identifying its capability gaps by using the AWS Cloud Adoption Framework (AWS CAF) perspectives.
Which phase of the cloud transformation journey includes these identification activities?

A. Envision
B. Align
C. Scale
D. Launch

B

(((118))) A company needs to perform data processing once a week that typically takes about 5 hours to complete.
Which AWS service should the company use for this workload?

A. AWS Lambda
B. Amazon EC2
C. AWS CodeDeploy
D. AWS Wavelength

B. Amazon EC2

For a data processing workload that needs to run for a longer duration (5 hours in this case) once a week, Amazon EC2 (Elastic Compute Cloud) would be a suitable choice. EC2 allows you to launch virtual servers with the desired specifications and run your application for the required duration.

AWS Lambda (option A) is a serverless compute service that's designed for short-lived, event-triggered functions. It's not suitable for long-running processes, and there are time and resource limitations.

AWS CodeDeploy (option C) is a deployment service for deploying applications to Amazon EC2 instances or on-premises instances, but it's not meant for running the application itself.

AWS Wavelength (option D) is a service that provides low-latency compute and storage at the edge of the 5G network, and it's typically used for applications that require ultra-low latency, which doesn't seem to be a requirement for the weekly data processing job described here.

(((121))) A company plans to deploy containers on AWS. The company wants full control of the compute resources that host the containers. Which AWS service will meet these requirements?

A. Amazon Elastic Kubernetes Service (Amazon EKS)
B. AWS Fargate
C. Amazon EC2
D. Amazon Elastic Container Service (Amazon ECS)

D

(((124))) A company wants to block SQL injection attacks.
Which AWS service or feature should the company use to meet this requirement?

A. AWS WAF
B. Network ACLs
C. Security groups
D. AWS Certificate Manager (ACM)

A

(((125))) A company wants a unified tool to provide a consistent method to interact with AWS services.
Which AWS service or tool will meet this requirement?

A. AWS CLI
B. Amazon Elastic Container Service (Amazon ECS)
C. AWS Cloud9
D. AWS Virtual Private Network (AWS VPN)

A


Which AWS service is deployed to VPSs and provides protection from common network threats?
AWS Network Firewall
-->


<!--
A company requires an encrypted connection between the company's on-premises servers and AWS. The connection must use the company's existing internet connection.

Which solution will meet these requirements?

Site-to-Site VPN 

Site-to-Site VPN creates an encrypted network path between your on-premises network and your AWS Cloud network. This connection between your on-premises network and your AWS Cloud network uses the internet. 



*

Which credential components are required to gain programmatic access to an AWS account? (Select TWO.)

An access Key ID
A secret access key

* 

Which AWS service identifies security groups that allow unrestricted access to a user's AWS resources?

AWS Trusted Advisor
 Trusted Advisor checks security groups for rules that allow unrestricted access to a resource. Unrestricted access increases opportunities for malicious activity, such as hacking, denial-of-service attacks, or loss of data

* 

A company is hosting a static website from a single Amazon S3 bucket. 

Which AWS service will achieve lower latency and high transfer speeds?

Amazon CloudFront
CloudFront is a web service that speeds up the distribution of your static and dynamic web content, such as .html, .css, .js, and image files, to your users. Content is cached in edge locations. Content that is repeatedly accessed can be served from the edge locations instead of the source S3 bucket.

* 

A company wants to create a learning application for students. The learning application must give students the option to choose a button to have the text read out loud to them.

Which AWS machine learning service will meet this requirement?

Amazon Polly is a machine learning service that converts text to speech. This service provides the ability to read text out loud.

* 

A company has an on-premises Linux-based server with an Oracle database that runs on it. The company wants to migrate the database server to run on an Amazon EC2 instance in AWS.

Which service should the company use to complete the migration?

AWS Application Migration Service (AWS MGN)
AWS MGN is an automated lift-and-shift solution. This solution can migrate physical servers and any databases or applications that run on them to EC2 instances in AWS.

* 
A company is moving all of their development activities to AWS. The company wants a solution to store and manage their developers' source code.

Which AWS coding service will meet this requirement?

CodeCommit is a source code version control service. CodeCommit helps users store and manage developers' source code in AWS.

-->