# SNS

1. Send message to multiple receivers
2. Pub/Sub model
3. The event producers only sends messages to one SNS topic
4. Multiple event receivers (Subscriptions) as we want to listen to SNS topic notifications
5. Each subscriber to the topic will get all the messages unless a filter is applied
6. Upto 10 million subscriptions per topic
7. Subscribers can be
    1. SQS
    2. HTTP/HTTPS
    3. Lambda
    4. Emails
    5. SMS messages
    6. Mobile notifications
8. Producers
    1. Cloudwatch -> for alarms
    2. Auto Scaling groups notifications
    3. Amazon S3 -> on bucket events
    4. Cloudformation -> upon state changes eg failed to build
9. To publish
    1. Topic publish using the SDK
        1. Create a topic
        2. Create 1 or many subscription
        3. Publish to the topic
    2. Direct publish for mobile apps SDK
        1. Create a platform application
        2. Create a platform endpoint
        3. Publish to the platform endpoint
10. Security
    1. Encryption
        1. In-flight encryption using HTTPS API
        2. At-rest encryption using KMS keys
        3. Client side encryption if the client wants to perform encryption/decryption
    2. Access controls
        1. IAM policies
    3. SNS Access policies
        1. For cross account access to SNS topics
11. SNS + SQS Fanout pattern
    1. To send messages to multiple SQS queues
    2. Push once in SNS topic and receive in all SQS queues that are subscribers
    3. Fully decoupled and no data loss
    4. Ability to add more SQS subscribers over time
    5. SQS queue access policy must allow SNS to write
    6. S3 event to multiple queue
        1. For same combination of event type eg Create object and a prefix eg /myObject there is a limitation to have only 1 S3 event rule
        2. If you want to send same S3 event to many SQS queue use fanout pattern
    7. FIFO topic
        1. Can have SQS FIFO queues as subscribers
        2. Deduplication using a Deduplication ID and also ordering
        3. Fanout + ordering + deduplication
    8. Message filtering
        1. JSON policy used to filter messages sent to SNS topics subscriptions
        2. If not filter is applied then all messages will be received
    9. If you create FIFO topic then only FIFO queue can subscribe it. Also name of FIFO topic should end with “.fifo”
