# RDS & Cache

1. RDS is Relational database service which uses SQL as query language.
2. It is a managed service this means it allows you to created database in cloud that are managed by AWS
3. You can make DB publicly available or keep it private in VPC
4. Can have password or IAM DB authentication
5. Can export logs to cloud watch
6. Can enable deletion protection
7. Vendors
    1. Postgres
    2. MySQL
    3. MariaDB
    4. Oracle
    5. Microsoft SQL server
    6. Aurora —> AWS proprietary database 
8. Benefits
    1. Automated provisioning , OS patching
    2. Continuous backups and point in time restore
    3. Monitoring dashboards
    4. Read replicas for improved read performance
    5. Multi AZ for disaster recovery
    6. Maintenance windows for upgrades
    7. Scaling capability (vertical and horizontal)
    8. Storage backed by EBS (gp2 or io1)
9. Cons
    1. As it is a managed service you cannot SSH into your instances
10. RDS backups
    1. Auto enabled
    2. Daily full backup of database (during maintenance windows)
    3. Transaction logs are backed up every 5 minutes
        1. Ability to restore to any point in time
    4. 7 days retention policy but can be increased to max 35 days
11. DB snapshots
    1. Manually triggered backups by use
    2. Retention of backup for as long as you want
12. Storage auto scaling
    1. Helps increase storage on RDS DB instance dynamically when RDS detects you are running out of free storage
    2. You have to set Max storage threshold
    3. Auto increase if
        1. Free storage is less than 10% of allocated storage
        2. Low storage lasts at least 5 minutes
        3. 6 hours have passed since last modification
    4. Supports all RDS DB engines and useful for un-predecitve workloads
13. Read replicas
    1. To scale reads of application
    2. Upto 5 read replicas
    3. Can be within AZ, Cross AZ, cross region
    4. Replication is ASYNC so reads are eventually consistent
    5. Can also be promoted to its own database
    6. Applications must update connection string to leverage read replicas
    7. Use case
        1. To run reporting or analytics
        2. To make reads faster and reduce load on production main database
    8. Only for SELECT queries
    9. If read replica is in same region than no cost of inter AZ (cross AZ) transfer
    10. Cost for inter Region data transfer (Cross region)
14. Multi AZ
    1. Used for disaster recovery
    2. SYNC replication
    3. Synchronous replication to standby AZ
    4. We get 1 DNS name which helps in auto failover to standby instance
    5. We get increase availability
    6. Failover in case of loads of AZ, network, storage failure
    7. Not used for scaling
    8. No manual intervention in apps
    9. Read replicas can be setup as Multi AZ for disaster recovery
15. Single AZ to Multi AZ
    1. Zero downtime operation
    2. Just modify the database
    3. Internally Snapshot of main DB will be taken and restored to create new DB in another AZ and then sync replication will be enabled
16. Encryption & security
    1. At rest encryption
        1. Can encrypt master and read replicas using AWS KMS AES-256 encryption
        2. Can only be encrypted at launch time
        3. If master DB is not replicated then read replicas cannot be encrypted
        4. Transparent data encryption (TDE) is available only for oracle and SQL server
    2. In flight encryption
        1. Uses SSL
        2. To enforce SSL
            1. In postgreSQL —> rds.force_ssl =1 (Parameter groups) in AWS RDS console
            2. In MySQL —> Within DB run “GRANT USAGE ON *.* TO ‘mysqluser’@‘%’ REQUIRE SSL;”
    3. Encryptions options
        1. Snapshot of unencrypted DB will be unencrypted
        2. Snapshot of encrypted  DB will be encrypted
        3. Can copy snapshot into encrypted one
    4. To encrypt and un-encrypted RDS DB
        1. Create snapshot of unencrypted DB
        2. Copt snapshot and enable encryption
        3. Restore DB from encrypted snapshot
        4. Migrate application to new database and delete. Old database
    5. Network security
        1. DB are Usually deployed within a private subnet, not in a public subnet
        2. Leverages security groups- it controls which IP/security group can communicate with RDS
    6. Access management
        1. IAM policies help control who manage AAWS RDS
        2. Traditional username and password can be used to login
        3. IAM based authentication can be enabled for Postgre and MYSQL engines
            1. You need authentication token which can be obtained through IAM & RDA API calls
            2. Auth token is valid for 15 minutes
            3. Benefits
                1. Network In/Out must be encrypted using SSL
                2. IAM to centrally manage users instead of DB
                3. Can leverage IAM roles and EC2 instances profiles for easy integration
17. AWS responsibility
    1. No SSH access
    2. No manual DB patching
    3. No manual OS patching
    4. Now ay to audit underlying instance
18. Your responsibility
    1. Check ports/IP/security group inbound in DB’s SG
    2. In-database user creation and permissions or manage though IAM
    3. Creating database with or without public access
    4. Ensure parameter groups or DB is configured to only allow SSL connections
19. Aurora
    1. Compatible with Postgres and MYSQL 
    2. Cloud optimised —> 5X performance improvement over MYSQL and 3X over Postgres
    3. DB grows automatically in increments of 10GB upto 64TB
    4. Can have upto 15 replicas
    5. Failover is instantaneous. Its HA native
    6. 6 copies of your data across 3AZ
        1. 4 copies out of 6 for writes
        2. 3 copies out of 6 for reads
        3. Self healing with peer-to-peer replication
        4. Storage is striped across 100s of volumes
    7. 1 aurora instance takes writes (master)
    8. Failover for master is <30sec
    9. Master + upto 15 read replicas (auto scaling can be done)
    10. Supports cross region replication
    11. DB cluster
        1. Writer endpoint -> pointing to master
        2. Reader endpoint -> connection load balancing (at connection level) for read replicas
        3. Client connects to writer and reader endpoint
    12. Security
        1. Encryption at rest using KMS
        2. Auto backups, snapshots and replicas are also encrypted
        3. Encryption in flight using SSL
        4. Possibility to authenticate using IAM token (Same method as RDS)
        5. Cannot SSH
    13. Read replicas can scale automatically
    14. If read replicas have different sets of instances (eg: 2 db.r3 & 3 dg.r5 instances) we can create subsets as custom endpoints for subsets
        1. Generally after crating custom endpoint, we don’t use reader endpoint
    15. Aurora serverless
        1. Automated SB instantiation and auto scaling based on actual usage
        2. Good for infrequent, intermittent or unpredictable workloads
        3. No capacity planning needed
        4. Pay per second, can be cost. Effective
    16. Aurora multi master
        1. If you need immediate failover for write nodes (HA)
        2. Every node does R/W -> in contrast to promoting a RR as new master
    17. Global aurora
        1. Aurora cross region read replicas
            1. Useful for disaster recovery
            2. Simple to put in place
        2. Aurora global database (recommended)
            1. 1 primary region (read/write)
            2. Upto 5 secondary (read-only) region replication lag os <1 sec
            3. Upto 16 read replicas per secondary region
            4. Helps for decreasing latency
            5. Promoting another region for disaster recovery has RTO <1 min
    18. Aurora Machine learning
        1. ML based predictions to application via SQL
        2. Simple, optimised and secure integration between aurora and AWS Ml services
        3. Supported services
            1. Amazon Sagemaker (use with any ML model)
            2. Amazon Comprehend (for sentiment analysis)
        4. No need of ML experience
        5. Use
            1. Fraud detection
            2. Ads targeting 
            3. Sentiment analysis 
            4. Product recommendations
20. ElastiCache
    1. Used to get redis or memchached
    2. Caches are in-memory databases with really high performance and low latency
    3. Helps reduce load off database for read intensive workloads
    4. Helps make your application stateless (store session data in cache)
    5. It is a managed service
    6. involves heavy application code changes
    7. Can have encryption at rest and transit
    8. Redis VS memcahced
        1. Redis
            1. Multi AZ with auto failover
            2. Read replicas to scale reads and high availability
            3. Data durability using AOF persistence
            4. Backup and restore features
            5. Can be used for database and queue
            6. Need at least 1 replica to use multi AZ feature
        2. Memcahced
            1. Multi-node for partitioning of data (sharing)
            2. No high availability (replication)
            3. Non persistent
            4. No backup and restore
            5. Multi threaded architecture
    9. All caches 
        1. Do not support IAM authentication 
        2. IAM policies on ElastiCache are only for AWS api level security
    10. Redis Auth
        1. You can set a password/token when you create redis cluster (Extra level of security)
        2. Supports in flight encryption
    11. Memcached
        1. Supports SASL based authentication (advance)
    12. Patterns
        1. Lazy loading
            1. All the real data is cached
            2. Data can become stale
            3. Only load data if there is cache miss
        2. Write through
            1. Adds or update data in the cache when written to a DB 
            2. No stale data
        3. Session store
            1. Store temporary session data in cache
            2. Using TTL feature
    13. Use
        1. Redis
            1. Gaming leaderboards are computationally complex
            2. Redis sorted sets 
                1. guarantees both uniqueness and element ordering
                2. Each time a new element added, it is ranked in real time and then added in correct order
            3. 
