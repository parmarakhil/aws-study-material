# Kinesis

1. To collect, process and analyse streaming data in real time
2. Ingest realtime data such as application logs, website clickstreams, IOT data
3. Kinesis Data Streams 
    1. Capture, process and store data streams
    2. Has streams which are made of shards
        1. More shards means more throughput
    3. Producers can be
        1. Kinesis agent
        2. Applications
        3. Client
        4. SDK, KPL
    4. Producers send record made of
        1. Partition key
        2. Data blob upto 1 MB
    5. 1 MB/sec or 1000 msg/sec per shard
    6. Consumers can be
        1. Apps like KCL, SDK
        2. Lambda
        3. Kinesis data firehose
        4. Kinesis data analytics
    7. Consumer receive records
        1. Partition key
        2. Sequence number
        3. Data blob
    8. 2 MB/sec per shard all consumer
    9. Billing is per shard provisioned 
    10. Retention of Daya for 1 day to 365 day
    11. Ability to reprocess data
    12. Once data is inserted in Kinesis it can’t be deleted
    13. Data that shares the same partition goes to same shard
4. Kinesis Data Firehose
    1. Load data streams into AWS Data stores
    2. Store data to target destination
    3. Producers can be
        1. Kinesis agent
        2. Applications
        3. Client
        4. SDK, KPL
        5. Kinesis data streams
        6. Cloudwatch
        7. AWS IOT
    4. Lambda functions can be attached to do some modifications
    5. Firehose performs batch writes to destination
        1. Near real time performance because fo batching
    6. Destinations
        1. S3
        2. Redshift (copy to S3)
        3. ElasticSearch
        4. Splunk
        5. MongoDB
        6. DataDog
    7. All or failed data can be sent to S3 bucket
    8. Fully managed service
    9. Pay for data going through firehouse
    10. Near real time
        1. 60 seconds latency minimum for non full batches
        2. Or min 32 MB of data at a time
    11. Supports many data formats, conversions, transformations, compression
5. Kinesis Data Analytics
    1. Analyse data streams with SQL or apache link
    2. For SQL application
        1. Write own SQL statement to query
    3. Perform real-time analytics on kinesis streams using SQL
    4. Fully managed
    5. Automatic scaling
    6. Realtime analytics
    7. Pay for actual consumption rate
    8. Can crate streams out of real time queries 
    9. Use cases
        1. Time series analytics
        2. Real time dashboards
        3. Real ime metrics
6. Kinesis Video Streams 
    1. Capture, process and store video streams
7. Firehose VS streams
    1. Streams
        1. Streaming service for ingest at scale
        2. Write custom code (Producer/consumer)
        3. Read time
        4. Manage scaling (shard splitting/merging)
        5. Data storagre for 1 to 365 days
        6. Supports replay capability
    2. Firehose
        1. Load streaming data to S3/Redshift/ES
        2. Fully managed
        3. Near real time
        4. Automatic scaling
        5. No data storage
        6. No replay support
8. Ordering data into Kinesis
    1. The same key will always go to same shard
9. Ordering data into SQS
    1. SQS FIFO
        1. If you don’t use a group ID, messages are consumed in the order they are sent with only one consumer
    2. SQS standard
        1. There is no ordering
    3. Group ID is similar to partition key in kinesis
10. Kinesis VS SQS ordering
    1. Kinesis data streams
        1. The data is ordered within each shard
        2. No guarantee of data ordering across shards 
        3. Can receive upto 5 MB/s of data
    2. SQS FIFO
        1. You only have one SQS FIFO queue
        2. No of consumers = no of groups
        3. Upto 300 Msg/s or 3000 if batching
11. SNS Vs SQS Vs Kinesis
    1. SQS
        1. Consumer pulls ata
        2. Data is deleted after being consumed
        3. Can have multiple consumers
        4. No need to provision throughput
        5. Ordering only in FIFO
        6. Individual message delay capability
    2. SNS
        1. Pub/Sub model
        2. Push data to many subscribers
        3. Data is not persisted I.e is list if not delivered
        4. Upto 100000 topics
        5. No need to provision throughput
        6. Integrates with SQS for fanout pattern
        7. FIFO capability for SQS FIFO
    3. Kinesis
        1. Standard -> pull data - 2MB per shard
        2. Enhanced fan-out - push data - 2MB per shard per consumer
        3. Possibility to replay data
        4. Meant for real time big data analytics
        5. Ordering at shard level
        6. Data expires after X days
        7. Must provision throughput
12. Amazon MQ
    1. SQS, SNS are cloud-native using proprietary protocols
    2. For traditional applications on cloud without re-engineering 
    3. Amazon MQ = managed Apache ActiveMQ
    4. Doesn’t run at scale like SQS and SNS, runs on a dedicated machine, can run in HA with failover
        1. Uses EFS as backups storage
        2. Types
            1. Active
            2. Standby
    5. Has queue feature and topic feature
