# SQS

1. Simple Queue service
2. Producer (1 or many) sends messages to queue and consumer ( 1 or many) consumes/poll the messages
3. Message is produced using SDK (sendMessage API)
4. Consumers can be EC2 instances, servers, lambdas
5. The message is persisted in SQS until a consumer deletes it
6. Used for application decoupling
7. Standard Queue
    1. Unlimited throughput
    2. Retention of messages
        1. Default 4 days
        2. Max 14 days
    3. Low latency
    4. Limitation of 256kb per message sent
    5. You can have duplicate messages (at least once delivery)
    6. Messages can be out of order (Best effort message ordering)
8. To increase throughput of processing you can add multiple consumers
    1. This can be automated by using ASG when using EC2
    2. The metric used for scaling is “ApproximateNumberOfMessages” this is a custom metric to be created in cloudwatch
9. Use “purge” to delete all messages in queue
10. Security
    1. Encryption
        1. In-flight encryption using https api
        2. At rest encryption using KMS keys
        3. Client side encryption if the client wants to performs encryption/decryption itself
    2. Access controls
        1. IAM policies to regulate access
    3. SQS Access policies
        1. Similar to S3 bucket policies
        2. Useful for cross-account access to SNS queries
11. SQS queue access policies uses
    1. Cross account access
    2. Publish S3 event notification to SQS queue
12. SQS Message Visibility timeout
    1. After a message is polled by a consumer it becomes invisible to other consumer
    2. It helps in avoiding processing of same message by multiple consumers
    3. After the timeout period is ended and consumer is not able to process message, the message is made visible again
    4. Default visibility timeout is 30 seconds
    5. If a consumer knows that it needs more time to process the request, it can increase the timeout period by using ChangeMessageVisibility API
13. Dead letter queue
    1. If consumer fails to process messages multiple times and MaximumReceives threshold is exceeded then we can put the message in separate queue called Dead letter queue
    2. Those messages can later be reviewed and debugged
    3. Good to set retention to 14 days
14. SQS - Request Response Systems
    1. Requests put requests in queue and responder will consume it and then put the response into another queue
    2. Requester and responders will send requests (messages) with correlation ID and reply-to filed
    3. To implement this pattern use SQS Temporary queue client
        1. It leverages virtual queues instead of creating/deleting SQS queues which is cost effective
15. Delay queue
    1. Use to delay messages so that. Consumer don’t see messages immediately 
    2. Default is 0 sec. Max at 15 min
    3. Can set at queue level or at message level using DelaySeconds parameter
16. SQS - FIFO queue
    1. First In First Out queue
    2. Ordering of queue is maintained
    3. It has limited throughput, can be improved using batching
    4. Gives Exactly once send capability
    5. Messages are processed in order by the consumer
    6. Uses de-duplication ID
