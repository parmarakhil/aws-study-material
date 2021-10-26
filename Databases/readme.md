# Databases

1. Types
    1. RDBMS (SQL/OLTP)
        1. RDS, Aurora
        2. Great for joins, normalised data
    2. NoSQL 
        1. DynamoDB
        2. ElastiCache (key/value pairs)
        3. Neptune (graphs)
        4. No joins, No SQL to query
    3. Object Store
        1. S3 - big objects
        2. Glacier - for backups
    4. Data warehouse (SQL analytics/BI)
        1. Redshift - Online Analytical Processing OLAP
        2. Athena
    5. Search
        1. ElasticSearch
    6. Graphs
        1. Neptune
        2. Display relationships between data
2. RDS
    1. Managed PostgreSQL, MySQL, Oracle, SQL server
    2. Must provision an EC2 instance & EBS volume type and size
    3. Support read replica and Multi AZ
    4. Security through IAM, Security groups, KMS SSL in transit
    5. Backup, Snapshot, point in time restore
    6. Managed and scheduled infra
    7. Monitoring through cloud watch
    8. Use
        1. Store relational datasets
        2. Perform SQL queries
        3. Transactional insets
    9. Operations
        1. Small downtime when failover
        2. Scaling in read replicas/ Ec2 instances
        3. Restore EBS volumes implies manual intervention
        4. Application changes
    10. Security
        1. AWS responsible for OS security 
        2. We are responsible for setting up KMS, Security groups, IAM policies, authorising user in DB
    11. Reliability
        1. Multi AZ
        2. Failover in case of failure
    12. Performance
        1. Depends on EC2 instance type, EBS volume type 
        2. Storage auto-scaling & manual scaling
    13. Cost
        1. Pay per hour based on provisioned EC2 & EBS
3. Aurora
    1. Compatible with PosttgreSQL/ MYSQl
    2. Auto healing
    3. Data is stored in 6 replicas in 3 AZ
    4. Multi AZ, Auto scaling read replica
    5. Read replica can be globalDefine EC2 instance type for aurora instances
    6. Aurora serverless
        1. For unpredictable workloads
    7. Aurora Multi-master
        1. For continuous writes failover
    8. Operations
        1. Less operations
    9. Security
        1. AWS responsible for OS security 
        2. We are responsible for setting up KMS, Security groups, IAM policies, authorising user in DB
    10. Reliability
        1. Multi AZ, more than RDS
        2. Failover in case of failure
    11. Performance
        1. 5x performance
        2. Upto 15 read replica
    12. Cost
        1. Pay per hour based on EC2
        2. Amt of Storage
4. ElastiCache
    1. Managed Redis, memcached 
    2. In-memory data store
    3. Sub-millisecond latency
    4. Must provision EC2 instance type
    5. Support for Clustering
    6. Security through IAM,S security groups
    7. Backup, point in time restore
    8. Managed and scheduled maintenance
    9. Monitoring through cloud watch
    10. Uses
        1. Frequent reads, less write
        2. Key/value store
    11. Operation
        1. Same as RDS
    12. Security
        1. AWS responsible for OS security 
        2. We are responsible for setting up KMS, Security groups, IAM policies, users (Redis Auth)
    13. Reliability
        1. Multi AZ
    14. Performance
        1. Sub-millisecond performance
    15. Cost
        1. Pay per hour based on EC2 and storage usage
5. DynamoDB
    1. NoSQL, global Db
    2. Cloud native Was proprietary
    3. Serverless, provisioned capacity, on demand capacity
    4. Can replace elasticache as key/value store 
    5. Highly available, DAX for read cache
    6. Reads and writes are decoupled
    7. Reads can be eventually or strongly consistent
    8. Security , authentication and authorisation is done through IAM
    9. DynamoDB streams to integrate with lambda
        1. Global tables
    10. Backup/Restore
    11. Can only query on primary key, sort key or indexes
    12. Monitoring through cloud watch
    13. Use case
        1. Item size less than 100Kbs
        2. Distributed serverless cache
        3. Transactions capability
    14. Operations
        1. No operations needed
    15. Security
        1. Full security through IAM policies, KMS encryption,, SSL in-transit
    16. Reliability
        1. Multi AZ
        2. Failover in case of failure
    17. Performance
        1. Single digit milliseconds
        2. Can be used as cache, DAX
        3. Performance doesn’t degrade with inc in size
    18. Cost
6. S3
    1. Key/value store for objects
    2. Great for big objects
    3. Serverless, max object size is 5TB
    4. Strong consistency
    5. Tiers
        1. S3 standard, IA, one zone IA
    6. Versioning, encryption
    7. Security - IAM, bucket policies , ACL
    8. Use
        1. Static files
        2. Website hosting
    9. Operations
        1. No operations needed
    10. Security
        1. We are responsible for setting up KMS, bucket policies, IAM policies
    11. Reliability
        1. Multi AZ
        2. Cross region replication
    12. Performance
        1. Scales to thousands of reads/writes
    13. Cost
        1. Pay per storage usage and network cost
        2. Requests number
7. Athena
    1. SQL layer on top of S3
    2. Fully serverless
    3. Pay per query
    4. Output results. Back to S3
    5. Secured through IAM
    6. Use case
        1. One time SQL queries
        2. Serverless queries on S3
    7. Operations
        1. No operations needed
    8. Security
        1. IAM + S3
    9. Reliability
        1. Managed services
        2. Uses presto engine
    10. Performance
        1. Queries scale based on data size
    11. Cost
        1. Pay per query of data scanned
8. Redshift
    1. Based on PostgreSQL
    2. Not used for OLTP
    3. Used for OLAP
    4. 10x better performance
    5. Scales to PB of data
    6. Massively parallel query Execution (MPP)
    7. Pay as you go
    8. Has a SQL interface
    9. BI tools such as Quicksight or tableau
    10. From 1 node to 128 node, upto 160GB of dance per node
    11. Redshift spectrum
        1. Perform queries directly against S3
    12. Backup & restore
    13. Redshift enhanced VPC routing
        1. Copy/upload without public internet
    14. No multi-AZ mode
    15. Snapshots are point in time backups of a cluster, are incremental
    16. Can restore snapshots into a new cluster
    17. Automated snapshots
    18. You can configure to a automatically copy snapshots to another AWS region
    19. Loading data into redshift
        1. Kinesis data firehose
        2. S3 using copy command
        3. EC2 instance JDBC driver
    20. Redshift Spectrum
        1. Query data that is already in S3 without loading it
        2. Must have a redshift cluster to start the query
        3. The query is submitted to thousands of redshift spectrum nodes
    21. Operations
        1. Same like RDS
    22. Security
        1. IAM, VPC,KMS,SSL
    23. Reliability
        1. Auto healing
        2. Cross regions snapshot copy
    24. Performance
        1. 10x performances
    25. Cost
        1. Pay per node provisioned 
        2. 1/10th cost
    26. Vs Athena 
        1. Faster queries thanks to indexes
9. Glue
    1. Managed extract, transform and load (ETL) service
    2. Useful to prepare and transform data for analytics
    3. Fully serverless service
    4. Glue data catalog
        1. Catalog of all datasets
        2. Metadata
        3. Glue crawler crawls the DBs and writes metadata to glue data catalog
10. Neptune
    1. Fully managed graph database
    2. Use
        1. High relationship data
        2. Social networking
        3. Knowledge graphs (wikipedia)
    3. Highly available across 3Az, 15 read replica
    4. Point in time recovery
    5. Supports KMS encryption + HTTPS
    6. Operations
        1. Same like RDS
    7. Security
        1. IAM, VPC,KMS,SSL + IAM authentication
    8. Reliability
        1. Multi AZ
        2. Clustering
    9. Performance
        1. Clustering to inc performance
    10. Cost
        1. Pay per node
11. ElasticSearch
    1. You can search any field even partial matches
    2. Usage for big data
    3. Built-in integration for firehose, IOT
    4. Security through Cognito & IAM, KMS encryption
    5. Provision a cluster
    6. Operations
        1. Same like RDS
    7. Security
        1. IAM, VPC,KMS,SSL, Cognito
    8. Reliability
        1. Multi-AZ
        2. Clustering
    9. Performance
        1. Based on elastics each project
    10. Cost
        1. Pay per node provisioned 

