# Notes on AWS Services

## AWS Developer Certified Associate

### VPC - Virtual private cloud
* Virtual Data Center

### Direct Connect
* way to connet to AWS without public internet - uses direct fiber

### Route 53
* Amazons DNS service - 53 stands for default DNS service port

---

### EC2 - Elastic Compute Cloud
* Virtual Computers
* AMI - Amazon Machine Image - comes with mny required softwares like AWS cli, ruby , php...
* When creating EC2 - subnet mask can be assigned to any of the availability zone in the region
* Only one roll can be assigned for an EC2 machine
* user data can be entered which is a shell script if we want some script to be ran  after login - will be run as root user
```sh
#!/bin/bash
yum update -y
yum install httpd24 -y
service httpd start
```
* tagging instances are used for identification and more - for ex - env tagged as dev/prod, owner tagged as admin/department user and more
* make sure private key file is protected with 400 access (pem file)
* to find the default username for different EC2 images - try login with root (ssh root@<public_ec2_ip> -i pem file)
* ssh <public_ec2_ip> -l <user-name> -i <pem file path>

#### AWS Command line tool configuration
* aws cli should be configured with an IAM user with valid access assigned to it.
* aws <command> <subcommand> - ex aws s3 ls
* aws cli should be configured - using aws configure command
* default config files will be stored under ~/.aws/config file (only if credentials are not entered)
* if credentials are entered during aws configure - it creates credentials file which overrides config file
* edit config file with keys "aws_access_key_id", "aws_secret_access_key"
* entry in credentials file always overrides config file

```code()
config file
[default]
aws_access_key_id=<val>
aws_secret_access_key=<val>
region = us-west-1

[user1]
aws_access_key_id=<val>
aws_secret_access_key=<val>
region = us-west-1

credentials file
[default]
aws_access_key_id=<val>
aws_secret_access_key=<val>
region = us-west-1

[user1]
aws_access_key_id=<val>
aws_secret_access_key=<val>
region = us-west-1

```
* better not to save access and secret keys in EC2 like this - use roles instead
* if user role s assigned to EC2 instance while creation - then no need to do aws configure if we want to connect from one aws service to anothe (for ex AWS EC2 to AWS S3)
* If we have to connect to AWS resource from outside 9local development machine fro example) install aws cli in local machine and configure it to use corect access and secret keys

#### AWS Roles configuration


---

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

---
### IAM
* Identity and Access Management - user groups and roles controlling
* should enable MFA(Multi Factor Authentication) for root user
* All users created only gets no access if we do not assign them any roles or policies or groups
* Cannot switch **roles** after EC2 provisioning(running) - but **permissions** can be changed inside the role

#### Active Directory Federation Authentication with IAM
* It is possible to use federated login(SSO/LDAP)
* ADFS - Active Directory Federated server
* SAML - Secure Assertion Markup Language
* Flow
    * browser to ADFS server (either by user or by AWS redirection)
    * ADFS server asks for authentication
    * after auth by ADFS - SAML response sent by ADFS server
    * SAML - submitted to AWS **AssumeRoleWithSAML** - that returns a temp security credential and constructs signin url for AWS console
    * client url redirected to AWS console
    * AWS SAML signin endpoint [http://signin.aws.amazon.com/saml](http://signin.aws.amazon.com/saml)
    
#### Web Identity Federation (with Mobile Application)
* authentication using services like google/facebook/linkedin...
* ARN - Amazon Resource Name
* [Further Reading](https://aws.amazon.com/articles/4617974389850313)
* Flow
    * Authenticate with web identity provider (user triggers from Web Identity Federation PLayground)
        * accessToken, userID, expiresIn, signedRequest... and more will be sent back by web identity provider
    * Click - Call **AssumeRoleWithWebIdentity** and then proceed to step 3
        * This step retrieves temporary security credential (from AWS)
        * also it return other details like Access Key ID, Secret Access, Session Token
    * With above details it is possible to talk to AWS resources

* Access key Id and Secret access key are used for AWS cli to communicate to aws services.
* It can be always re created for every IAM user.

---    
        

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
