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
  * Cn sustain 1 facility disasted.
* S3-RR TIER (Reduced Redundancy)
  * 99.99 % availability and 99.99% durability.
  * 99.99999999999%(11 9's) durability
* Glacier - Archival storage
  * 3-5 hrs to restore from glaciers
* 1 byte to 5TB for a file - unlimited storage
* https://s3-<region_code>.amazonaws.com/<bucket_name> - buckets are universally global - should be unique.
* read after write consistency for new objects - eventual consistency for overwrite and delete operations.
* charged based on storage size, requests, data transfer
```
