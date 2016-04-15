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



#### EC2 Roles configuration
* Assign role during EC2 creation
* policies in the rule can be modified even if the EC2 instance is running
* Disk encryption applied @ EBS level

#### EC2 instance metadata
* (more to read)[http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html]
* http://169.254.169.254/latest/meta-data
* if user data (boot script) is included during instance creation that also can be viewed
* if user data is modified through console  - it reflects only aftere restarting the instance


---

### AWS SDK
* android, iOS, javascrip(browser)
* Java, python, .net, nodeJS, nodejs, ruby, php, Go, C++
* some SDKs have default regions like java - it is us-east-1  nodeJS do not have default region
* setup region in SDK code

---


### ELB - Elastic Load Balancer
* Can have multiple SSL certificates on an ELB
* Multiple ssl certificates can be terminated at once an ELB - like terminating all SSL certificates on an ELB.
* Not Free - on hour and per GB usage
* Cloud Formation, Elastic BEanstalk, Autoscaling are free - results of these services are not free
* ELB can use these ports - SMTP:25, http:80, https:448 and then 1024~65535
* HTTP Codes
```code()
200 - request success
404 - not found
500 - INternal Server Error
3xx - redirection
4xx - client error
404 - not found
500 - server error
Read more about http codes
```


---

### ECS - EC2 Container Service
* For docker containers running in EC2 cluster

### EBS - Elastic beans talk
* to deploy apps developed in any language and docker too - nginx, apache, IIS ... supported


### Lamda
* Code ececution service - no other services or machines required
* only to pay execution time of our code


### S3 - Simple Storage Storage
* Object based storage - everything stored in buckets
* All buckets are private by default
* can have only 100 buckets pee AWS account - further can be requested by submitting request 
* Can have ACL (Access Control Lists) in granular level like role/user/bucket/file based 
* Integrates well with IAM when EC2 talsk to S3
* S3 integrates well with cloud front  
* Cannot rename bucket - delete and re create is the option 
* allows upload parts of file (multipart file) - useful for large files
* > 100 mb file is better if uploaded with multipart - if it > __5GB__ should be uploaded through multipart
* Allows stop/resume file upload   
* all the copied files are synced between availability zone in that region (might be a little (nano seconds) delay in sync up) 
* offers API - aws cli
* web services interface to store and retrieve files
* size __1byte to 5TB__ - unlimited storage
* Files in S3 are called as objects - stored under buckets - bucket name should be unique across the region (basically it is a namespace)
* 99.9% availability and 99.999999999% durability in standard mode
* 99.9% availability and 99.99% durability in reduced redundancy storage mode
* can have metadata(key value pair on each files)
* supports lifecycle maintenance and versioning (versionning records deletion as well)
* versioning cannot be disabled once enabled. can be suspended
* Lifecycle management can be used with/without verioning - and can be applied to current/previous versions of S3 file
     * Archive Only
     * Permanently Delete only
     * Archive and then permanently delete  
* supports encryption
    * Uses AES 256 - Advanced Encryption Standard 256
    * Data is secured with X5098 certificates
    * upload/download vie SSL encrypted endpoint
    * supports server side encryption - keys can be managed by S3 itself of AKM (AWS Key Management) or privately
    * can do  client side encryption : client can encrypt the data and encrypted data can be stored in S3
* S3 buckets has multiple settings like permissions,static site hosting, logging, Event notification, lifecycle , tags, versining, CORS, 
    Requestor pays (will force the dowloader to pay for AWS bandwith usage)
* for static web site - should mention index document and error document        
* end point for static site will be shown immediately. format below
    * Ex : black-gate-static-site.s3-website-us-west-1.amazonaws.com
    * format : __<bucket_name>.s3-website-<region>.amazonaws.com__   
    
#### S3 CORS (Cross Origin Resource Sharing)
* CORS should be configured in destination resourec - from where /how it can be accessed.
```code()
<?xml version="1.0" encoding="UTF-8"?>
<CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
    <CORSRule>
        <AllowedOrigin>ORIGIN S3 BUCKET URL HERE</AllowedOrigin>
        <AllowedMethod>GET</AllowedMethod>
        <MaxAgeSeconds>3000</MaxAgeSeconds>
        <AllowedHeader>*</AllowedHeader>
    </CORSRule>
</CORSConfiguration>                                             
```                                             

### S3 - versioning and Lifecycle management
* If versining enable it cannot be disable - only it can be suspended - or the bucket should be deleted - cost can be controlled by lifecycle
* can lead to cost increase as every update is stored as bucket version
* every version of the file is maintained - if same file uploaded again it still counts as 2 versions
* REST api available for v ersioned file as well
    * Ex : https://<file_url>?versionId=<version_id>
* Life cycle management enabled with or without versioning
* mainly used with versioning - Ex : moving older version files to glacier 
* Lifecycle management can be enabled for whole bucket or specic folder/object in a bucket
* Lifecycle actions include below (__check in AWS for versioned and non-vrsioned buckets__)
    * Archive Only - Ex : Move to glacier after N days
    * Expire - Ex : Expire the file after N Days (if not versioned - it is equivalant to delete)
    * Archive and expire - Ex : Archive to Glacier after N days and then expire after N+M Days
    * Transition to infrequent access storage
    * Permanently deleting - older version of files
    * Cleaning up partial multipart uploads
* Expire for versioned object means - moving current to previous version and place a delta marker in current version - to delete permanentlt(versioned objects) should combine
     expire on current version and permanently delete the previous version
* if versioning was enabled - these above rules can be applied to older versions and new versions of files differently
* Naming a rule is optional    


---                                              
                                                                                                                                          
#### Glacier
* Data archieval service  - cheaper than S3 (standard and reduced redundancy mode) - .01$/month/GB
* storage anm retrieval is generally longer than S3 - 3-5 hrs are best match
* store and add files to vaults - supports data retrieval policies and event notofications
---

### Cloud Front
* Amazon CDN - Cloud delivery Network - deliver static files from edge locations (dynamic files also can be cached - not files but the static content associated with the url actually)
* Cloud front is optimized to work with other services like S3, Route 53, EC2, ELB
* Terminology
    * Origin - origin of a file - can be s3, ELB or route 53
    * Distribution - collection of edge locations
* One distribution with multiple origin is possible
* Distribution Types : 
    * Web Distribution - web apps - can be used for live steeaming also
    * RTMP - MEdia Streaming (files must be in S3)
#### Creation of distribution liost
* Origin can be either S3 or ELB     
* cloud front can restrict S3 public access (Restrict Bucket Access menu)
* with path pattern - can specifi multiple origins based on path - for Ex : *.jpef - go to S3, *.php - go to ELB...
* Viewer protocol policy below are possible
    * HTTP, HTTPS only
    * Redirect HTTP to HTTPS
    * HTTPS only
* Allowed HTTP Methods
    * GET, HEAD
    * GET, HEAD, OPTIONS
    * GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE
* Cached HTTP Methods
    * can cache only specific methods from above -for example - can cache OPTIONS method if required
* Forward Headers
    * Forwarding all or only whitelisted headers to origin
* Object Caching
    * cloud front can use origin's cache headers 
* TTL 
    * Applicable only if above setting is custon - dont want to use origin's cache headers
    * set values in seconds - minimum maximum, default
    * Minimum TTL is 300 seconds
* Forward Cookies
    * Forwarding all or only whitelisted cookies to origin
* Forward Query Strings
    * ask cloud front to include the query string
* Restrict Viewer Access
    * Rsstrict viewer acces based on signed url (AWS accout #s can be specified)
* Compress Objects Automaticall
    * Enables automatic gzip comression 
* Smooth Streaming - used for microsoft streaming services
*

* Price class 
    * All
    * US Europe
    * US Eurpe and Asia
* Alternate Domain Names - can specifi custon name instead of CDN name (*.cloudfront.net)
* If want to use own SSL certificate we can use - or we can stick with default cloudfront SSL cert.
* Default Root Object - default landing page - can enable logging also    
---

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
* Cloud database service (oracle, MS SQL, mySQL, postgres, aurora)

### Dynamo DB 
* No SQL from AWS - supports Document and key-value data models
* Similar to MongoDB - but cannot have custom(Embedded) data types.
* SSD storage and repliocated across __3__ facilities (data centers).
* COnsistency model
    * Eventually Consistent Reads(Default) - eventually will be available in other 2 facilities - since replication lags some milliseconds. generally happens withing a second. (no specific SLA from documentation)
    * Strongly COnsistent Reads - available in other 2 facilities before any read operations from any other read operation from anu other facilities.
```json
{
"_id": ObjectId("271864abvkahjbfvskjhfbvalkq97yqaifgjb"),
"fname": "fnameeee",
"lname": "lnamebsdjfh",
"address": {            //    not possible - embedded
        "street": "sdfbsdf",
        "zip": 123456
    }
}
```
* Pricing  
    * TRhroughput - .0065$/Hour for every 10 units write and every 50 units read
    * storage - .25$ per month
```code()
//Pricing Example
App requires 1 million reads and 1 million writes per day and storing 3GB of data
11.6 writes/ second and 11.6 reads/second = 12 read capacity units and 12 write capacity units
12 read units would cost .0374 and 12 write capacity units would cost .1872 and in total - .2246 per day
Storage is .25/GB/month so - .025$/day
total cost is .025 + .2246 = .2496$/day
```      
* nice to have a role if EC2 wants to interact with Dynamo DB
* Indexes always is made up of one or two attributes (hash or hash and range)
* Primary key attributes (Primary Index)
    * HAsh key or partition key - Index will automatically be craeted on this hash key (if PK composed of only hash attribute) - check for latest version 
    * Hash key + Range Key (Sort Key) (like a composite key)
        * __Un ordered index on the hash  - ordered/sorted index on the range/sort key__
    * each attributes cannot exceed 64 kb in size    
* Secondary Index 
    * Local Secondary Index
        * Same hash key and different range key.
        * can only be created during table creation - cannot be modified after table creation
    * Global Secondary Index    
        * Different hash key and different range key
        * This can be created during tabel creation or can be added later
* __5__  locala and __5__ global secoondary indexes are the limit per table.
           
#### Query  vs Scan
* Query operatioin should contain primary key based search(hash key must be provided)
* Query can have range as optional attribute
* By default Query returns all the column (including hash attribute) - can use __Projection expression__ if we want to limit the columns
* Query results are always sorted by range ky - ascending (numeric or ascii  char code values) - to reverse it __set ScanIUndexForward__ param to false.
* By default Query is eventually consisten - but can be configured to strongly consistent
* Scan checks every item in table - return all column/attributes in the table but can use __Projection expression__ if we want to limit attributes.
* Inefficient - as it scans the entire table
* Avoid using scans on large tables
* scan becomes more slow when tabel grows. It might eat the all throughput of the table quickly.
* __USe Query, Get, BatchGetItem__ APIs than scan.      
           
          
---
    
### Elasticache
* In Memory Cache service for data from AWS (used with AWD DB services)
* supports 2 in-memory open source implementations - memcached and redis cache engines

### Redshift
* Data Warehousing Service (Business Intelligence DB Server) - OLAP database


### DMS
* Database Migration Service - used for migration 

### EMR
* Elastic analysing of big data

### Data pipeline
* moving data from one service to another

### Elastic Search Service
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
 * Go through __FAQ__ section of each area targetted for the exam
* Go through COde Commit, Code Deploy, Code Pipeline
* If you want to enable a user to download your private data directly from S3, you can insert a pre-signed URL into a web page before giving it to your user. --> TRUE


