* Check udemy video annotations too.
* check for region places and its code

### S3
```sh
* simple key value store - object based. Every object consists of key, value, versionId, metadata, subresources, Access Control Lists
* different storage tiers - al tiers latency in milliseconds (except glacier)
* S3-99.99 TIER - default
  * can sustain 2 facilities disaster.
  * 99.99% availability - 99.99999999999%(11 9's) durability
* S3-IA TIER (Infrequently accessed)
  * for less frequent data access- lower than S3-99.99% but with freq based retrieval cost
  * very low retrieval time - fast
  * 99.9% availability 99.99999999999%(11 9's) durability
  * Cn sustain 1 facility disaster.
* S3-RR TIER (Reduced Redundancy)
  * 99.99 % availability and 99.99% durability.
  * 99.99999999999%(11 9's) durability
* Glacier - Archival storage
  * 3-5 hrs to restore from glaciers
* storage tiers and encryptions (storage classes) can be applied @ individual object level - not necessarily need to be in bucket level.  
* 1 byte to 5TB for a file - unlimited storage
* https://s3-<region_code>.amazonaws.com/<bucket_name> - buckets are universally global - should be unique.
* https://s3-<region_code>.amazonaws.com/<bucket_name>/<file_name>?versionId=<version_id>
* read after write consistency for new objects - eventual consistency for overwrite and delete operations.
* charged based on storage size, requests, data transfer
* versioning must be enabled(in source and destination) for cross region replication
* a role/policy is required for cross region replication.
* Life cycle management
 * atleast 30 days wait requiored for moving from "standard to IA" or "IA to glacier"
 * file size must be atleast 128kb in size
 * glacier charges based on 90 days period. so preferance to keep it for 90 days atleast.
 * with verioning to delete a file permanently - do this
  * expire the current ve - i.e - apply delete marker on current version
  * then delete previous versions on file
 * Event notifications help doing something in S3 events like put/delete/..
  * can pass event to SNS, SQS, lamda (lamda is based on region support)
* S3 security
 * bucket policies - bucket level control
 * Access Control Lists(ACLs) - provide item level access control inside a bucket
 * Data transfer security
  * uses https SSL/TLS
 * At rest security
  * Server Side Encryption - SSE
   * SSE-S3 - S3 managed keys (AES256 - Advanced Encryption Standard)
   * SSE-KMS - Amazon Key Management service
   * SSE-C - Customer provided keys
  * Client side encryption
   * Encrypt the data in client side and upload it
  
```
### cloud front
```sh
```
