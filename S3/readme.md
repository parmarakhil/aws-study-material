# S3

1. Infinitely scaling storage
2. Buckets are region scoped 
3. Buckets must have a globally unique name
4. S3 is a rest based service
5. Naming convention
    1. No uppercase
    2. No underscore
    3. 3063 characters long
    4. Not an IP
    5. Must start with a letter or number
6. Object has a key. A key is the path of object (excluding S3://<bucketName>)
7. Key is composed of prefix+objectName eg(myfolder/subfolder/abc.txt here abc.txt is object name)
8. There is no concept of directories
9. Object values are content of body
    1. Max object size is 5TB
    2. If you are uploading file > 5GB you have to use multi-part upload
10. Metadata is present which is either list of text key/value pairs - system or user metadata)
11. Tags for security and lifecycle policy
12. You can enable versioning also
    1. Versioning is enabled at bucket level
    2. Uploading object with same key will be create a new version
    3. Benefits
        1. Protection against unintended delete because you can restore a version
        2. Easy rollback to previous version
    4. Any file that didn’t had a version prior to enabling versioning will have version “Null”
    5. If you suspend versioning it will not delete previous versions
13. If bucket is not public they you cannot open object with a public url
    1. As a workaround S3 provides a pre-signed URL which contains a temporary credentials using which you can view the file
    2. A pre-signed url is valid for 3600 seconds, can change timeout with —expires-in arguments
    3. Users who are given a pre-signed URL inherit the permissions of user who created the URL for GET/PUT
    4. Use
        1. Allow only logged-in use to download premium video on S3 bucket
        2. Allow an ever changing list of users to download files by generating URLs dynamically
        3. Allow temporarily a user to upload a file to a. Precise location in our bucket
14. Encryption for objects
    1. SSE-S3
        1. Encrypts S3 objects using keys handled and managed by AWS S3
        2. Object is encrypted server side using AES-256 encryption type
        3. Must set header : 
            1. “x-amx-server-side-encryption”:”AES256”
    2. SSE-KMS
        1. Leverages AWS KMS to manage encryption
        2. Keys are handled and managed by KMS
        3. Gives control over keys (who has access )+ audit control
        4. Object is encrypted server side
        5. Must set header : 
            1. “x-amx-server-side-encryption”:”aws:kms”
    3. SSE-C
        1. Mange your encryption keys on your own I.e you manage the keys
        2. HTTPS must be used (Mandatory)
        3. S3 does not store encryption key you provide
        4. Encryption key must be provided in HTTP header for every HTTP request made
        5. To retrieve object you will have to share same key used to encrypt
        6. Can only be done through CLI 
    4. Client side encryption
        1. Encryption and decryption is done at client side
        2. AWS has client library (S3 encryption client) to help in this
        3. Customer manages keys and encryption cycle
    5. We can specify encryption at both object level and bucket level
15. S3 Security
    1. User based
        1. IAM policies
            1. Specifies which API calls should be allowed for a specific user from IAM console
    2. Resource based
        1. Bucket policies
            1. Bucket wide rules from S3 console
            2. Allows cross account
        2. Object Access control List (ACL)
        3. Bucket Access control list (ACL)
    3. Note:
        1. An IAM principal can access an S3 object if
            1. The user IAM permissions allow it or the resource policy Allows it and there is no explicit deny
16. S3 bucket policies
    1. JSON based policies
        1. Can be applied to buckets or objects
        2. Actions —> set of API to allow or deny
        3. Effect —> allow/deny
        4. Principal —> the account or user to apply the policy to
    2. Use
        1. Grant public access to bucket
        2. Force object to be encrypted at upload
        3. Grant access to another account (cross account)
    3. Bucket settings for block public access
        1. New access control list
        2. Any access control list
        3. New public bucket or access point policies
    4. Block public and cross account access to buckets and objects through any public bucket or access point policies
    5. Can be set at account level
    6. Networking
        1. Supports VPC endpoints
    7. Logging and audit
        1. S3 access logs can be stored in other S3 bucket
        2. API calls can be logged in cloudtrail
    8. User security
        1. MFA delete: MFA for delete operations
        2. Pre signed URL that are valid only for a limited time
17. S3 static websites
    1. S3 allows to host static website
    2. If you get 403 (forbidden error) make sure the bucket policy allows public reads
    3. You need to disable block all public access and add a bucket policy to allow public access
18. S3 CORS
    1. Stands for Cross origin resource sharing
    2. An origin is a scheme (protocol), host(domain) and port
    3. Web browser based mechanism to allow requests to other origin while visiting the main origin
    4. We can fulfil CORS requests by adding CORS headers (eg: Access-Control-Allow-Origin)
    5. This header is set at the cross origin (child server)
    6. In S3 context it has to be allowed at the S3 bucket (cross bucket) which will be accessed as a child from main bucket (1st bucket)
19. S3 consistency model
    1. Strong consistency
    2. After write of a new object or overwrite or delete you will get latest version
    3. No additional cost or performance impact
20. MFA delete
    1. To enforce user to use generated code to do important operations
    2. To use MFA you need to enable versioning on S3 bucket
    3. You will need MFA to
        1. Permanently delete an object version
        2. Suspend versioning on bucket
    4. No need MFA for
        1. Enabling versioning
        2. Listing deleted versions
    5. Only the bucket owner (mostly root account) can enable/disable MFA delete
    6. Can be enabled using the CLI
21. Default encryption
    1. You can for encryption using bucket policies
    2. Bucket polices are evaluated before default encryption
    3. If an encrypted file is uploaded then default encryption will encrypt it
22. S3 Access logs
    1. If you want logs of any request made to S3 from any account, authorised or denied then we can log it into another S3 bucket
    2. If you store logs in same bucket, the bucket size will grow exponentially
        1. Do not set your bucket to be monitored bucket
    3. Logs can be analysed later by using Athena or any other tool
23. S3 replication
    1. Must enable versioning in both source and destination
    2. Cross region replication (CRR)
        1. Compliance
        2. Low latency access
        3. Replication across accounts
    3. Same region replication (SRR)
        1. Log aggregation
        2. Live replication between production and test accounts
    4. Buckets can be in different accounts
    5. Copying is asynchronous
    6. Must give proper IAM permissions to S3
    7. After activating replication only new objects are replicated
    8. For delete operations
        1. Can replicate delete markers from source to target
        2. Deletions of version ID are not replicated
    9. There is no chaining of replication
        1. If bucket 1 has replication into bucket 2 and bucket 2 has replication into bucket 3 then changes made in bucket 1 will not be reflected into bucket 3
24. S3 storage classes
    1. Amazon s3 standard -> general purpose
        1. High durability 11 9s of objects across multiple AZ
        2. Can sustain 2 concurrent facility failures
    2. Amazon S3 standard -> infrequent Access (IA)
        1. Suitable for data that is less frequently accessed but require rapid access when needed
        2. Low cost compared to Amazon S3 standard
        3. Can sustain 2 concurrent facility failures
        4. Use case
            1. Ass a data store for disaster recovery
    3. Amazon S3 One zone infrequent Access
        1. Same as IA but data is stored in single AZ
        2. Data is lost when AZ is destroyed
        3. Low latency and high throughput performance
        4. Supports SSL for data at transit and encryption at rest
        5. Low cost compared to IA
        6. Use case
            1. Storing secondary backup copies
            2. Data that you can re-create
    4. Amazon S3 Intelligent reining
        1. Small monthly and auto tiering fee
        2. Auto moves objects between two access tiers
    5. Amazon Glacier
        1. Low cost object storage meant for archiving
        2. Data is retained for longer term
        3. Alternative to on-permise magnetic tape storage
        4. Each item in glacier is called Archive
        5. Archives are stored in Vaults
    6. Amazon glacier deep archive
        1. Amazon glacier
            1. Expedited -> 1 to 5 hours
            2. Standard -> 3 to 5 hours
            3. Bulk -> 5 to 12 hout
            4. Min storage duration of 90 days
        2. Deep archive
            1. For longer term
            2. Standard -> 12 hours
            3. Bulk -> 48 hours
            4. Min storage duration of 180 days
25. Lifecycle rules
    1. Transition actions
        1. It defines when objects are transitioned to another storage class
            1. Move objects to Standard IA class 60 days after creation
            2. Move to Glacier for achieving after 6 months
    2. Expiration actions
        1. To delete objects after sometime
            1. Access log files can be set to delete after 1 year
            2. Can be used to delete old versions of files if versioning is enabled
            3. Can be used to delete incomplete multi-part uploads
    3. Can be created for certain prefix (eg: S3://mybucket/folder/*)
    4. Can be applied to tagged files also
26. S3 Analytics
    1. To determine when to transition objects from standard to standard IA
    2. Does not work for one zone IA or glacier
    3. Report is updated daily
    4. Takes 24h to 48h to first start
    5. Good step to put together lifecycle policies as it helps understand pattern
27. Performance
    1. Latency 100-200ms
    2. At-least 3500 PUT/COPY/POST/DELETE and 5500 GET/HEAD requests per second per prefix in a bucket
    3. KMS limitations
        1. If you use KMS, performance can web impacted
        2. On upload it calls GenerateDataKey KMS Api
        3. When download, Decrypt KMS api
    4. Multi part upload for files > 5GB
        1. Can help parallelise uploads
        2. Recommended for files >100MB
    5. S3 transfer acceleration
        1. To increase transfer speed by transferring file to AWS edge location which will forward data to S3 bucket int target region
        2. Compatible with multi part upload
    6. S3 byte range fetches
        1. Parallelise GETs by request specific byte ranges
        2. Better resilience in case of failures
        3. Can be used to speed up downloads
        4. Can be used to retrieve only part of data eg head of file
28. S3 select & Glacier select
    1. Retrieve less data using SQL by performance server side filtering
    2. Can filter rows and columns
    3. Less network transfer, less CPU cost client side
    4. Can’t do complex queries
29. S3 event notifications
    1. Ability to react on S3 events 
    2. Object name filtering possible
    3. Use case
        1. Generate thumbnails of images uploaded to S3
    4. Targets
        1. SNS
        2. SQS
        3. Lambda
    5. Can create as many S3 events as desired
    6. If versioning is not enabled and same object is uploaded multiple times then some notifications can be missed
30. S3 requester pays
    1. In general the bucket owner pay for all bucket storage and data transfer cost
    2. With requester pays buckets, the requester will pay for data transfer cost (networking cost) when performing data download
    3. The owner still pays for storage inside the bucket
    4. The requester must be authenticated in AWS (cannot be anonymous)
31. S3 Athena
    1. Use to perform analytics directly on S3 data without moving data out of the bucket
    2. It is a server less service
    3. Uses SQL language to query the files
    4. Charged per query and about of data scanned
    5. Supports CSV, JSON, ORV
    6. Built on Presto technology
32. S3 lock policies and glacier vault lock
    1. Glacier vault lock
        1. Adapt a WORM (Write once read many) model
        2. Lock the policy for future edits (can no longer be changed)
        3. Helpful for compliance and data retention
    2. S3 object lock
        1. Versioning must be enabled 
        2. Adapt a WORM (Write once read many) model
        3. Object retention
            1. Retention period
                1. Specifies a fixed period
            2. Legal hold
                1. Same protection, no expiry data
        4. Modes
            1. Governance mode
                1. User’s can’t override or delete an object version or alter its lock settings unless they have special permissions
            2. Compliance mode
                1. A protected object version can’t be overwritten or deleted by any user including the root user in your AWS account
	
