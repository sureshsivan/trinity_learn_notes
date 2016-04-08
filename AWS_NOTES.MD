# Notes on AWS Services

## AWS Developer Certified Associate

### VPC - Virtual private cloud
* Virtual Data Center

### Direct Connect
* way to connet to AWS without public internet - uses direct fiber

### Route 53
* Amazons DNS service - 53 stands for default DNS service port

### EC2 - Elastic Compute Clour
* Virtual Computers

### ECS - EC2 Container Service
* For docker containers running in EC2 cluster

### EBS - Elastic beans talk
* to deploy apps developed in any language and docker too - nginx, apache, IIS ... supported


### Lamda
* Code ececution service - no other services or machines required
* only to pay execution time of our code


### S3 - Simple Storage Storage
* Object based storage
* offers API

### Cloud Frong
* Amazon CDN - Cloud delivery Network

### EFS - Elastic File System
* Like a NAS in the cloud - can be connected to multiple EC2 
* Block level Storage


### EBS
* Virtual hard disk service from amazom

### Snowball
* Import export service - when moving large data to Amazon cloud
* physical hard disk case sent to location through UPS

### Storage Gateway
* Gateway between cloud and organization network

### RDS - Relational Database Servide
* Cloud database service (oracle, MS SQL...)

### Dynamo DB 
* No SQL from AWS

### Elasticache
* Cache service for data from AWS (used with AWD DB services)
* memcache and redis cache engines

### Redshift
* Data Warehousing Service (Business Intelligence DB Server)

### Glacier
* Data archieval service

### DMS
* Database Migration Service - used for migration 

### EMR
* Elastic analysing of big data

### Data pipeline
* moving data from one service to another

### Elastic Service
* Elastic search - in real time - does log analytics/real time monitoring/ click strream analytics and more

### Kinesis
* Streaming data - channel for streaming

### Machine Learning
* AWS service for MAchine Learning


### Quick Sight
* Business Intelligent Service (like cognos) - does adhoc analysis/visualizatons and more


### IAM
* Identity and Access Management - user groups and roles controlling
* should enable MFA(Multi Factor Authentication) for root user
* All users created only gets no access if we do not assign them any roles or policies or groups

### Directory Services


### Inspector
* Install agents in EC2 - used for security vulnarability inspection

### WAF 
* Web app firewall service

### Cloud HSM
* Hardware Securing Module - securing cloud resources

### KMS - Key Management Service


### Cloud Watch
* Monitoring tool for AWS Environment  

### Cloud Formation
* Script out infrastructure - datacenter can bve scripted

### Cloud Trail
* Auditing for AWS Environment - all AWS events are logged


### Ops Works 
* config and amanagenet service for application using chef


### AWS Config
* SOurce inventory, SCM, notofications
* can define rules and make sure - like SCM


### Service Cataloge
* Create and Manage Service Catalogue

### Trusted Advisor
* Scans our cloud env and provide options to save money, increase security and more..
* 2 tiers - free and business tiers

### API Gateway
* Create publish, monitor API services - used by developer

### AppStrea
* Streaming applications services from amazon
* Apps deployed and rendered to AWS infrastructure and output directed to end user devices

### Cloud Search
* Managed Service -search solution in the cloud
* has geospatial search

### Elastic Transcoder Service
* Easy to use transcoding service in the cloud
* transcoding media files from one format to other (mp4 to AVI.....)

### SES - Simple Email Service
* Cloud Service for send/receive email

### SNS - Simple Notification Service
* 

### SQS - Simple Queue Service
* decoupling infrastructure
* The first service launched by AWS

### SWF - Simple Workflow Service
* to design and execute workflows in the cloud(*like batches) - sequential or parallel

### Code Commit
* Source code maintenance and versioning system

### Code Deploy
* Automates code deployment to any AWS service - like jenkins

### Code Pipeline
* Countinuous devlivery stsyem

* All above 3 is AWS version of github

### Mobile Hub
* way of test and monitor usage of mobile app

### Cognito
* way of saving of saving mobile data

### Device Farm
* cloud test for smart phones and tables - devices run in cloud

### Mobile Analytics
* MEasure app revenue and more analytics

### Workspaces
* Virtual Desktops in the cloud (VDIs)

### Workdocs
* Enterprise document storage service

### Workmail
* Exchange email service from amazon

### IOT - Internet of Things
* COnnecting real time devices to cloud 



# TODO - Before Exam
* Go through COde Commit, Code Deploy, Code Pipeline
