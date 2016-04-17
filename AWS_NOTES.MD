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
* SSD storage and repliocated across __3__ facilities (data centers). (might fall under 1 or 2 availability zones)
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
           

#### Provision and throughput calculation
* Read Throughput - Unit definition
    * Every read is considered as __4kb ad its order__. For ex, 5kb considered as 8kb an 11 kb considerd as 12 kb.
    * Eventually consistent(default) 2 reads per second - strong consistent reads 1 read per second.
* Write Throughput - Unit definition
    * All writes are considerd as 1kb and its order
    * all writes are i write/second.
* Examples
    * Question : App that requires to read 10 items of 1kb/second using eventual consistency.
        * Read Units per item = (1kb/4kb) = 0.25 -> round to 1.0
        * total read units for all item = 1.0 * 10 = 10 read units
        * total read units considering consistency = 10/2 = 5 (as eventual consistency allows 2 reads per second)
        * read throughput = 5 units
    * Question : App that requires to read 10 items of 6kb/second using eventual consistency.
        * Read Units per item = (6kb/4kb) = 1.5 -> round to 2.0
        * total read units for all item = 2.0 * 10 = 20 read units
        * total read units considering consistency = 20/2 = 10 (as eventual consistency allows 2 reads per second)
        * read throughput = 10 units
    * Question : App that requires to read 5 items of 10kb/second using eventual consistency.
        * Read Units per item = (10kb/4kb) = 2.5 -> round to 3.0
        * total read units for all item = 3.0 * 5 = 15 read units
        * total read units considering consistency = 15/2 = 7.5 = 8 (ceil) (as eventual consistency allows 2 reads per second)
        * read throughput = 8 units
    * Question : App that requires to read 5 items of 10kb/second using strong consistency.
        * Read Units per item = (10kb/4kb) = 2.5 -> round to 3.0
        * total read units for all item = 3.0 * 5 = 15 read units
        * total read units considering consistency = 15/1 = 15 (as eventual consistency allows 2 reads per second)
        * read throughput = 15 units
    * Question : App that requires to write 5 items of 10kb/second using strong consistency.
        * Write Units per item =  = 2.5 -> round to 10
        * total write units for all item = 10 * 5 = 50 read units
        * write throughput = 50 units

#### API Error Code
    * 400 HTTP Status code - ProvisionedThroughputExceededException
        * means exceeded the provisioned throughput on tables or __one or more global or secondary indexes__.


#### Web Identity providers - Open ID providers
    * Uses __AssumeRoleWithWebIdentity API__ from AWS.
    * Steps
        * Authenticates with ID provider like facebook or google
        * They will be provided a token
        * application code should use __AssumeRoleWithWebIdentity API__ with passed token and ARN(Amazon Resource Name) for the IAM(Identity and Access Management) role
            * role should be created in IAM - with table access or just read access  -- and more (ARN of the Dynamo DB table should be mentioned while creating the role policy)
        * Then app can access dynamo DB from __15 mins to 1 hr - default 1 hr__.

#### Conditional Writes : 
    * Conditional Writes are idempotent - re executing the query will not do the update again
    
#### Atomic counters
    * Supports increment/decrement attribute values withouit interfering other writes.
    * Atomic counter is not idempotent - re executing the query will make update.
    
#### Batch Operations
    * USefule when the app wants to read multiple  items - __BatchGetItem__ API
    * __BatchGetItem__ (__Not BatchGetItemS__) can retrieve upto 1MB of data which can contain as many as 100 items. and it can retrieve from multiple tables.

#### Exporting data from Dynam DB
    * export to csv - is possible

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
* Diagram : http://docs.aws.amazon.com/sns/latest/dg/SNSMobilePush.html
* Notofication service which uses pub-sub (publish-subscribe) model (push mechanism) - eliminates the need to periodic poll.
* can push notification to mobile devices or as sms or email or to SQS or to any http endpoint.
* all published messages are stored across multiple availability zones (it prevents messages to lost)
* TOPIC:topic is an access points for subscription and the place to publish the mnessage
* any no of subscribers and any type of subscribers (mobile push/sms/email) can subscribe to a topic - SNS delivers the message to different type os subscribers by firmatting it
* price : 0.50$ per million SNS requests and 0.06$ per 100k deliveries over http, 0.75$ per 100 sms, 2.00$ per 100K email
* __data types is JSON__
* Subscription expires in 3 days if not confirmed
* TTL - Time To Live in seconds - if undelivered - message will expire
* Protocols : HTTP, HTTPS, email, EMail-JSON, SMS, SQS, Lambda, Application
* MEssage can be customized for each protocol

```json
Sample Message
{
  "Type" : "Notification",
  "MessageId" : "de400cf7-5127-5373-be49-437baa6d2824",
  "TopicArn" : "arn:aws:sns:us-west-1:143095841095:testtopic",
  "Subject" : "TEst Subject 1",
  "Message" : "TEst Raw MEssage 1",
  "Timestamp" : "2016-04-17T02:46:34.043Z",
  "SignatureVersion" : "1",
  "Signature" : "S/oQxfxugokT441MsQsCRSWQaI7XEw89WO81E3WEexjOr1ISSdm6CMUHNm5Jzqg1aqMMy41krZayv8iNLt2l+ULhFduAdF1JrhbKdchaTJFmdUy2DvCk5vbcst7TyNqGGDTuePtjKwl2OhwlxtIhaCIK7/ZN4BIRL2o5dU/kUMKj6YwG5B0or1RqVkO3ya/mz27EhJm5orKJfJ0dFdJ0QthNGj96Yj7t4WEoVAH7CVltquWk3BUA+5bhbgN8b/DX/s8tpzGC6obnZq7NQdso7xNEOX6VfV8sU8vCd8QktylZBr7//m1xuZYyAW2/AIjUNGi286e9M47MUdjTU4Q4Bg==",
  "SigningCertURL" : "https://sns.us-west-1.amazonaws.com/SimpleNotificationService-bb750dd426d95ee9390147a5624348ee.pem",
  "UnsubscribeURL" : "https://sns.us-west-1.amazonaws.com/?Action=Unsubscribe&SubscriptionArn=arn:aws:sns:us-west-1:143095841095:testtopic:68c0ff4c-e59e-413d-a919-cafe3055f2a8",
  "MessageAttributes" : {
    "AWS.SNS.MOBILE.MPNS.Type" : {"Type":"String","Value":"token"},
    "AWS.SNS.MOBILE.MPNS.NotificationClass" : {"Type":"String","Value":"realtime"},
    "AWS.SNS.MOBILE.WNS.Type" : {"Type":"String","Value":"wns/badge"}
  }
}
```

### SQS - Simple Queue Service
 * Diagram : http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/MessageLifecycle.html
* decoupling infrastructure components of AWS - producer consumer pattern
* The first service launched by AWS
* Web service message Queue which can be used to store messages waiting for computers to process them - queue is a temporary repository for the messages waiting to be processed
* __256 KB__ of message can be stored in failsafe queue - billing @ 64 kb chunks.
* multiple producers and consumers can be linked to one queue
* Does not guarantee __FIFO__ - if needed that should be handled separately 
* Ensures delivery of each message atleast once - after polling message will not be deleted/removed automatically from the queue - consumer has to delete it from the queue
* If consumer fails to delete in a timeframe - the message will start be visible to other consumers
* messages are not pushed by queue - consumer has to __poll__.
* Visibility timeout check happens only when message is polled
* supports autoscaling - if queue capacity threshold reached (some %) - can spin up additional consumers and add to the queue.
* __default visibility timeout is 30 seconds - maximum is - 12 hr__
 * timeout can be changed (if 30 sec is not sufficient), by __ChangeVisibilityAction__ / __ChangeMEssageVisibility__ - then SQS restarts the timeout period with new value
* single SQS request can have upto __10 message with maximum of 256 kb paylod__ - billed @ 64 kb chunk
    * one message which contains 8 messages and 256 KB in total size is billed as 4 requests.
* load and priority is generally handled by creating multiple queues - with different no of consumers in those queues.
* Traditional SQS polling returns with message immediately
* __Long Polling__ can be used which would wait for a message to arrive in the queue and return or wait till the timeout and return no message
* Max long poll timeout is 20 seconds - long poll saves some money by minimizing the requests.
* SQS FAnning out
    * passing one message to multiple queues
    * create SNS topic and create multiple SQS Queues which subscribes to that SNS topic
    * any message sent to SNS will published to all those queues

### SWF - Simple Workflow Service
* to design and execute workflows in the cloud(*like batches) - sequential or parallel
* co ordinates work across distributed components
* workflows can invoked by executable code/web service calls/script/human actions...
* amazon.com uses SWF - for order workflow
* SWF Workers : programs that interactt with SWF for receive tasks and process it  then return results
* SWF Deciders : co ordibbates the tasks
* SWF brokers interaction b/w workers and decidors - ensures task is assigned only once and not duplicated.
    keeps track of task executions
* Workers and decidors run independently and no need to keep track of tasks - SWF does the orchestration
* SWF Domains : Domain isolate set of types executions and task list from others withing same account
* Domains can be registered using __RegisterDomain__ action in SWF - parameters are specified as JSON.
    one of the argument is "workflowExecutionRetentionPeriodInDays"
* Message validity on SWF is __1 year__ and measured in seconds (__14 days in SQS - default is 4 days__)
* SWF is task oriented API (SQS is message oriented)
* SWF ensures task is assigned only once no duplication- in SQS it should be handled by consumer
* SWF keeps track of all tasks - SQS should do it in program especially when multiple queues
* diagram : http://docs.aws.amazon.com/amazonswf/latest/developerguide/swf-dev-workflow-exec-lifecycle.html

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
* Go through all the QA sections in the tutorial


``` code()
answer : B and C
Which statements about DynamoDB are true? (Pick 2 correct answers)
A. DynamoDB uses a pessimistic locking model
B. DynamoDB uses optimistic concurrency control
C. DynamoDB uses conditional writes for consistency
D. DynamoDB restricts item access during reads
E. DynamoDB restricts item access during writes
```
```code()
Which of the following are elements in an Amazon Simple Workflow Service (SWF) workflow?
Choose 3 answers
A.Workflow activity
B.Decider
C.Activity worker
D.Adapter
E.Message port
F.Workflow starter
Answer: BCF

```