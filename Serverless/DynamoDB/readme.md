# DynamoDB
 
1. DynamoDB
    1. Fully managed DB
    2. Replication across 3AZ
    3. NoSQL database
    4. Scales to massive workloads, distributed database
    5. Millions of requests per seconds
    6. Fast and consistent performance
    7. Integrated with IAM for security 
    8. Enables event driven programming using streams
    9. DynamoDB is made of tables
    10. Each table has a primary key
    11. Each table has infinite items
    12. Max size of items is 400Kb
    13. Provisioned throughput
        1. Table must have provisioned read and write capacity unit
        2. RCU
            1. Throughput for reads
            2. 1 RC = 1 strongly consistent read of 4KB per sec
            3. 1 RC = 2 eventually consistent read of 4KB per sec
        3. WCU
            1. Throughput for writes
            2. 1 WCU -> 1 write of 1 KB per second
        4. Option to setup. Auto scaling of throughput
        5. You can use burst credit. If burst credit is empty you will get “ProvisionedThroughputException”
        6. Exponential back-off for retry
    14. DAX
        1. DynamoDB Accelerator
        2. Seamless cache for DynamoDB
        3. Writes go through DAX to DynamoDB
        4. Micros second latency for cached reads
        5. Solves the Hot key problem (too many reads)
        6. 5 minutes TTL for cache by default
        7. Upto 10 nodes in cluster
        8. Multi-AZ
        9. Secure
    15. Streams
        1. Changes in dynamoDB can end up in DynamoDB streams
        2. This stream can be read by AWWS lambda and later perform operations
        3. Could implement cross region replication using streams
        4. Stream has 24 hours of data retention
    16. Transactions
        1. All or nothing type of operations
        2. Co-ordinate across multiple tables
        3. Include upto 10 unique items or upto 4 MB of data
    17. On Demand
        1. No capacity planning - scales automatically
        2. 2.5x more expensive
        3. Helpful when spikes or very less reads
    18. Security
        1. VPC endpoints available to access DynamoDB without internet
        2. Access fully controlled by IAM
        3. Encryption t rest using KMS
        4. Encryption in transit using SSL/TLS
    19. Backup and restore
        1. Point in time restore
    20. Global tables
        1. Active active replication, many regions
        2. Must enable DynmoDB streams to use it
        3. Useful for low latency and disaster recovery purposes
    21. DMS to migrate to DynamoDB
    22. Can launch local DynamoDB for development
