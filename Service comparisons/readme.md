# Comparisons

1. VPC Peering vs PrivateLink
    1. These 2 developed separately, but have more recently found themselves intertwined.
        1. VPC Peering - applies to VPC
        2. PrivateLink - applies to Application/Service
    2. With VPC Peering you connect your VPC to another VPC. Both VPC owners are involved in setting up this connection. When one VPC, (the visiting) wants to access a resource on the other (the visited), the connection need not go through the internet.
    3. VPC peering allows VPC resources including ... to communicate with each other using private IP addresses, without requiring gateways, VPN connections, or separate network appliances. ...Traffic always stays on the global AWS backbone, and never traverses the public internet
    4. Inter-Region VPC Peering provides a simple and cost-effective way to share resources between regions or replicate data for geographic redundancy.
    5. PrivateLink provides a convenient way to connect to applications/services by name with added security. You configure your application/service in your VPC as an AWS PrivateLink-powered service (referred to as an endpoint service). AWS generates a specific DNS hostname for the service. Other AWS principals can create a connection to your endpoint service after you grant them permission.
    6. When you create a VPC endpoint service, AWS generates endpoint-specific DNS hostnames that you can use to communicate with the service. These names include the VPC endpoint ID, the Availability Zone name and Region Name, for example, vpce-1234-abcdev-us-east-1.vpce-svc-123345.us-east-1.vpce.amazonaws.com. By default, your consumers access the service with that DNS name
    7. When you create an endpoint, you can attach an endpoint policy to it that controls access to the related service
    8. An endpoint policy does not override or replace IAM user policies or service-specific policies (such as S3 bucket policies). It is a separate policy for controlling access from the endpoint to the specified service.
2. Transit Gateway Vs VPC Peering
    1. Transit Gateway solves the complexity involved with creating and managing multiple VPC peering connections at scale. While this makes TGW a good default for most network architectures, VPC peering is still a valid choice due to the following advantages it has over TGW:
        * Lower cost — With VPC peering you only pay for data transfer charges. Transit Gateway has an hourly charge per attachment in addition to the data transfer fees.
        * No bandwidth limits — With Transit Gateway, Maximum bandwidth (burst) per VPC connection is 50 Gbps. VPC peering has no aggregate bandwidth. Individual instance network performance limits and flow limits (10 Gbps within a placement group and 5 Gbps otherwise) apply to both options. Only VPC peering supports placement groups.
        * Latency — Unlike VPC peering, Transit Gateway is an additional hop between VPCs.
        * Security Groups compatibility — Security groups referencing works with intra-Region VPC peering. It does not currently work with Transit Gateway.
    2. Within your Landing Zone setup, VPC Peering can be used in combination with the hub and spoke model enabled by Transit Gateway.
    3. VPC Peering,
        * Network connections between two VPCs
        * Can connect VPCs in different AWS account.
        * The AWS accounts can be in different AWS region(Inter-Region VPC Peering)
        1. The Good Bites.
            * Lowest cost options
            * Pay only for data transfer
            * No aggregate bandwidth limit.
        2. Things to look out for
            * VPC to VPC connections
            * No transit routing
            * Complex at scale
            * Max 125 connections per VPC
    4. Transit Gateway,
        * Network connections between 1000's of VPC
        * Remove complexity managing multiple connections
        * Hub and spoke design for connecting VPC together
        1. The Good Bites
            * Simplified management of VPC connections.
            * AWS manages the auto scaling and avability needs
            * Supported 1000's of connections
        2. Things to look out for
            * Hourly charges per attachments in addition to data fees
            * Max bandwidth burst to 50 Gbps
            * Infra-region security groups don't support Transit Gateway
    5. AWS Best Practice
        1. AWS VPC Peering
            * VPC peering should be used when the number of VPC's to be connected is less than 10.
            * There is a Max limit 125 peering connections per VPC.
        2. AWS Transit Gateway
            * One transit gateway in a given region
            * Place transit gateway in Network Service Account
            * Use AWS Resource access manager to share a transit gateway for connecting VPCs access multiple account
3. DataSync Vs Storage gateway
    1. One is for optimized data movement (DataSync), and the other is more suitable for hybrid architecture (Storage gateway).
        * AWS DataSync is ideal for online data transfers. You can use DataSync to migrate active data to AWS, transfer data to the cloud for analysis and processing, archive data to free up on-premises storage capacity, or replicate data to AWS for business continuity.
        * AWS Storage Gateway is a hybrid cloud storage service that gives you on-premises access to virtually unlimited cloud storage.
    2. You can combine both services. Use AWS DataSync to migrate existing data to Amazon S3, and then use the File Gateway configuration of AWS Storage Gateway to retain access to the migrated data and ongoing updates from your on-premises file-based applications.
4. Cloudwatch vs Cloudtrail
    1. CloudWatch is a monitoring service for AWS resources and applications. CloudTrail is a web service that records API activity in your AWS account. They are both useful monitoring tools in AWS.
    2. By default, CloudWatch offers free basic monitoring for your resources, such as EC2 instances, EBS volumes, and RDS DB instances. CloudTrail is also enabled by default when you create your AWS account.
    3. With CloudWatch, you can collect and track metrics, collect and monitor log files, and set alarms. CloudTrail, on the other hand, logs information on who made a request, the services used, the actions performed, parameters for the actions, and the response elements returned by the AWS service. CloudTrail Logs are then stored in an S3 bucket or a CloudWatch Logs log group that you specify.
    4. You can enable detailed monitoring from your AWS resources to send metric data to CloudWatch more frequently, with an additional cost.
    5. CloudTrail delivers one free copy of management event logs for each AWS region. Management events include management operations performed on resources in your AWS account, such as when a user logs in to your account. Logging data events are charged. Data events include resource operations performed on or within the resource itself, such as S3 object-level API activity or Lambda function execution activity.
    6. CloudTrail helps you ensure compliance and regulatory standards.
    7. CloudWatch Events is a near real time stream of system events describing changes to your AWS resources. CloudTrail focuses more on AWS API calls made in your AWS account.
    8. Typically, CloudTrail delivers an event within 15 minutes of the API call. CloudWatch delivers metric data in 5 minutes periods for basic monitoring and 1 minute periods for detailed monitoring. The CloudWatch Logs Agent will send log data every five seconds by default.
5. Security group Vs Network Access Control
    1. Security groups 
        1. Acts as a firewall for Associated EC2 instances
        2. Controls both inbound and outbound traffic at the instance level
        3. You can secure your VPC instances using only security groups
        4. Supports allow rules only
        5. It is stateful
        6. Has separate inbound and outbound traffic
        7. Evaluates all rules before deciding whether to allow or Deny traffic
        8. A newly created security group denies all inbound traffic by default and allow all outbound traffic by default
        9. Security groups are associated with network interfaces
    2. Network access control list
        1. Acts as a firewall for associated subnets
        2. Controls both inbound and outbound traffic at the subnet level
        3. They are an additional layer of security
        4. They are stateless
        5. Has separate rules for inbound and outbound traffic
        6. A newly created nACL denies all inbound traffic by default and also deny all outbound traffic by default
        7. Each subnet in your VPC must be associated with a network ACL. If none is associated, the Default NACL is selected
        8. You can associate a network ACL with multiple subnets however a subnet can be associated with only one network ACL at a time
6. IAM policies VS Service Control Policies (SCP)
    1. SCP are mainly used along with AWS Organisation while IAM policies operate at principal level
    2. SCPs do not replace IAM policies, they do not provide actual permission like IAM policies. IAM policies are of two types
        1. Identity based policies - attached to an IAM user, group or role
        2. Resource based policies - attached to an AWS resource like S3 bucket
    3. SCP takes precedence over IAM policies. Even if a principal is allowed to perform an action by IAM policy, an attached SCP can overrode that capability
    4. IAM policies cannot be attached to AWS Organisation
    5. In SCP you choose to either whitelist or blacklist a specific AWS service while an IAM policy can allow or deny actions. Access to any services that isn’t explicitly allowed by the SCP is denied to the AWS accounts associated with SCP. In IAM policy, an explicit allow overrides an implicit deny and an explicit deny overrides an explicit allow
7. Data pipeline Vs database migration service
    1. AWS Data Pipeline helps you to process and move data (ETL) between S3, RDS, DynamoDB, EMR, On-premise data sources.
        1. AWS Data Pipeline can create complex data processing workloads that are fault tolerant, repeatable, and highly available
        2. AWS Data Pipeline launches required resources and tear them down after execution
        3. REMEMBER : AWS Data Pipeline is NOT for streaming data!
    2. AWS Database Migration Service is used to migrate databases to AWS while keeping source databases operational.
        1. Two types of migrations:
            1. Homogeneous Migrations (ex: Oracle to Oracle)
            2. Heterogeneous Migrations (ex: Oracle to Amazon Aurora, MySQL to Amazon Aurora)
        2. important characteristics:
            1. Free for first 6 months when migrating to Aurora, Redshift or DynamoDB
            2. (AFTER MIGRATION) You can keep databases in sync and pick right moment to switch
        3. Important use cases:
            1. Consolidate multiple databases into a single target database
            2. Continuous Data Replication can be used for Disaster Recovery
8. Aurora Vs Dynamodb
    1. Aurora has concept of referential integrity I.e foreign keys while DynamoDB doesn’t have that
    2. Aurora only has immediate consistency while in DynamoDB you can have eventual as well as immediate consistency
    3. Aurora is a relational DBMS model while DynamoDB is Document store and key-value store
    4. Aurora supports server-side scripting while dynamodb  doesn’t
    5. In Aurora, portioning can be done with horizontal partitioning. DynamoDB supports sharing as partitioning method
    6. Aurora supports SQL query language while dynamodb doesn’t
    7. Aurora supports on one replication method - master-slave replication. DynamoDB supports replication methods
    8. Aurora doesn’t offer API for user-defined Map/Reduce methods. DynamoDb also does not offers but maybe implemented via Amazon Elastic MapReduce
9. Scaling policies
    1. Simple Scaling 
        1. Simple scaling relies on a metric as a basis for scaling. 
        2. For example, you can set a CloudWatch alarm to have a CPU Utilization threshold of 80%, and then set the scaling policy to add 20% more capacity to your Auto Scaling group by launching new instances. Accordingly, you can also set a CloudWatch alarm to have a CPU utilization threshold of 30%. When the threshold is met, the Auto Scaling group will remove 20% of its capacity by terminating EC2 instances. 
    2. Target tracking
        1. Target tracking policy lets you specify a scaling metric and metric value that your auto scaling group should maintain at all times. 
        2. Let’s say for example your scaling metric is the average CPU utilization of your EC2 auto scaling instances, and that their average should always be 80%. When CloudWatch detects that the average CPU utilization is beyond 80%, it will trigger your target tracking policy to scale out the auto scaling group to meet this target utilization.
        3. this type of policy assumes that it should scale out your Auto Scaling group when the specified metric is above the target value.
        4. You cannot use a target tracking scaling policy to scale out your Auto Scaling group when the specified metric is below the target value.
        5. The Auto Scaling group scales out proportionally to the metric as fast as it can, but scales in more gradually. 
        6. Lastly, you can use AWS predefined metrics for your target tracking policy, or you can use other available CloudWatch metrics (native and custom). Predefined metrics include the following
            1. ASGAverageCPUUtilization
            2. ASGAverageNetworkIn
            3. ASGAverageNetworkOut 
            4. ALBRequestCountPerTarget
    3. Step Scaling
        1. Step Scaling further improves the features of simple scaling. Step scaling applies “step adjustments” which means you can set multiple actions to vary the scaling depending on the size of the alarm breach. 
        2. When a scaling event happens on simple scaling, the policy must wait for the health checks to complete and the cooldown to expire before responding to an additional alarm. This causes a delay in increasing capacity especially when there is a sudden surge of traffic on your application.
        3. With step scaling, the policy can continue to respond to additional alarms even in the middle of the scaling event.
10. Parameter store Vs Secrets Manager
    1. Parameter store is part of the application management tools offered by the AWS Systems Manager (SSM) service. Parameter Store allows you to create key-value parameters to save your application configurations, custom environment variables, product keys, and credentials on a single interface. 
    2. Parameter Store allows you to secure your data by encryption which is integrated with AWS KMS.
    3. Secrets manager enables you to rotate, manage, and retrieve database credentials, API keys and other secrets throughout their lifecycle. It also makes it really easy for you to follow security best practices such as encrypting secrets and rotating these regularly. 
    4. Secrets Manager can offload the management of secrets from developers such as database passwords or API keys, so they don’t have to worry about where to store these credentials.
    5. Parameter Store was designed to cater to a wider use case, not just secrets or passwords, but also application configuration variables like URLs, DB hostnames, custom settings, product keys, etc. which is why the default selection for creating a parameter is a plain text String value. You can enable encryption if you explicitly choose to. 
    6. Secrets Manager was designed specifically for confidential information that needs to be encrypted, which is why encryption is always enabled when you create a secret. You can’t store data in plaintext in Secrets Manager.
    7. Secrets Manager also provides a built-in password generator through the use of AWS CLI.
11. SQS Vs Kinesis
    1. Amazon Kinesis Data Streams enables real-time processing of streaming big data. It provides ordering of records, as well as the ability to read and/or replay records in the same order to multiple Amazon Kinesis Applications. The Amazon Kinesis Client Library (KCL) delivers all records for a given partition key to the same record processor, making it easier to build multiple applications reading from the same Amazon Kinesis data stream (for example, to perform counting, aggregation, and filtering).
    2. Amazon Simple Queue Service (Amazon SQS) offers a reliable, highly scalable hosted queue for storing messages as they travel between computers. Amazon SQS lets you easily move data between distributed application components and helps you build applications in which messages are processed independently (with message-level ack/fail semantics), such as automated workflows.
    3. Amazon Kinesis Data Streams for use cases with requirements that are similar to the following:
        1. Routing related records to the same record processor (as in streaming MapReduce). For example, counting and aggregation are simpler when all records for a given key are routed to the same record processor.
        2. Ordering of records. For example, you want to transfer log data from the application host to the processing/archival host while maintaining the order of log statements.
        3. Ability for multiple applications to consume the same stream concurrently. For example, you have one application that updates a real-time dashboard and another that archives data to Amazon Redshift. You want both applications to consume data from the same stream concurrently and independently.
        4. Ability to consume records in the same order a few hours later. For example, you have a billing application and an audit application that runs a few hours behind the billing application. Because Amazon Kinesis Data Streams stores data for up to 365 days, you can run the audit application up to 365 days behind the billing application.
    4. Amazon SQS for use cases with requirements that are similar to the following:
        1. Messaging semantics (such as message-level ack/fail) and visibility timeout. For example, you have a queue of work items and want to track the successful completion of each item independently. Amazon SQS tracks the ack/fail, so the application does not have to maintain a persistent checkpoint/cursor. Amazon SQS will delete acked messages and redeliver failed messages after a configured visibility timeout.
        2. Individual message delay. For example, you have a job queue and need to schedule individual jobs with a delay. With Amazon SQS, you can configure individual messages to have a delay of up to 15 minutes.
        3. Dynamically increasing concurrency/throughput at read time. For example, you have a work queue and want to add more readers until the backlog is cleared. With Amazon Kinesis Data Streams, you can scale up to a sufficient number of shards (note, however, that you'll need to provision enough shards ahead of time).
        4. Leveraging Amazon SQS’s ability to scale transparently. For example, you buffer requests and the load changes as a result of occasional load spikes or the natural growth of your business. Because each buffered request can be processed independently, Amazon SQS can scale transparently to handle the load without any provisioning instructions from you.
12. EFS Vs FSx for lustre
    1. EFS is is accessed by EC2 instances running inside one of your VPCs. Instances connect to a file system by using a network interface called a mount target. Each mount target has an IP address, which we assign automatically or you can specify.
    2. Amazon EFS is designed to be highly durable and highly available. With Amazon EFS, there is no minimum fee or setup costs, and you pay only for what you use.
    3. Amazon FSx for Lustre makes it easy and cost-effective to launch and run the world’s most popular high-performance file system. 
    4. Allows your workloads to process data with consistent sub-millisecond latencies, up to hundreds of gigabytes per second of throughput, and up to millions of IOPS.
    5. POSIX-compliant, so you can use your current Linux-based applications without having to make any changes, providing a native file system interface that works as any file system does with your Linux operating system.
    6. File system data is automatically encrypted at-rest and in-transit.
    7. Amazon FSx for Windows File Server provides simple, fully managed, highly reliable file storage that’s accessible over the industry-standard Server Message Block (SMB) protocol.	
    8. Provides file storage that is accessible from Windows, Linux, and MacOS compute instances and devices running on AWS or on-premises.
13. CloudFront Vs Global Accelerator 
    1. CloudFront uses multiple sets of dynamically changing IP addresses while Global Accelerator will provide you a set of static IP addresses as a fixed entry point to your applications.
    2. CloudFront pricing is mainly based on data transfer out and HTTP requests while Global Accelerator charges a fixed hourly fee and an incremental charge over your standard Data Transfer rates, also called a Data Transfer-Premium fee (DT-Premium).
    3. CloudFront uses Edge Locations to cache content while Global Accelerator uses Edge Locations to find an optimal pathway to the nearest regional endpoint.
    4. CloudFront is designed to handle HTTP protocol meanwhile Global Accelerator is best used for both HTTP and non-HTTP protocols such as TCP and UDP. 
    5. Global Accelerator is used when for example you have a banking application that is scattered through multiple AWS regions and low latency is a must. Global Accelerator will route the user to the nearest edge location then route it to the nearest regional endpoint where your applications are hosted. 
    6. CloudFront is used when for example you have some static content to be served to your users, in this case the content will be cached at the edge location and requests will be served from the cached edge location instead of your source region. This provides content with low latency
14. Cognito User pools Vs Identity Pools
    1. Amazon Cognito User Pools are used for authentication. You  have a way for to login using username/passwords or federated login using Identity Providers such as Amazon, Facebook, Google, or a SAML supported authentication such as Microsoft Active Directory.
    2. With Cognito User Pools, you can provide sign-up and sign-in functionality for your mobile or web app users. 
    3. You don’t have to build or maintain any server infrastructure on which users will authenticate. 
    4. you can even use the pre-built login UI provided by Amazon Cognito which you just have to integrate on your application.
    5. Cognito Identity Pools (Federated Identities) provides different functionality compared to User Pools.
    6.  Identity Pools are used for User Authorization. You can create unique identities for your users and federate them with your identity providers.
    7. Using identity pools, users can obtain temporary AWS credentials to access other AWS services. 
    8. Identity Pools can be thought of as the actual mechanism authorizing access to AWS resources. 
    9. When you create Identity Pools, think of it as defining who is allowed to get AWS credentials and use those credentials to access AWS resources.
    10. You can define rules in Cognito Identity Pools for mapping users to different IAM roles to provide fine-grain permissions.
15. S3 Vs Glacier
    1. Amazon S3 is a durable, secure, simple, and fast storage service, while Amazon S3 Glacier is used for archiving solutions.
    2. Use S3 if you need low latency or frequent access to your data. Use S3 Glacier for low storage cost, and you do not require millisecond access to your data.
    3. You have three retrieval options when it comes to Glacier, each varying in the cost and speed it retrieves an object for you. You retrieve data in milliseconds from S3.
    4. Both S3 and Glacier are designed for durability of 99.999999999% of objects across multiple Availability Zones.
    5. S3 and Glacier are designed for availability of 99.99%.
    6. S3 can be used to host static web content, while Glacier cannot.
    7. In S3, users create buckets. In Glacier, users create archives and vaults.
    8. You can store a virtually unlimited amount of data in both S3 and Glacier.
    9. A single Glacier archive can contain 40TB of data.
    10. S3 supports Versioning.
    11. You can run analytics and querying on S3.
    12. You can configure a lifecycle policy for your S3 objects to automatically transfer them to Glacier. You can also upload objects directly to either S3 or Glacier.
    13. S3 Standard-IA and One Zone-IA have a minimum capacity charge per object of 128KB. Glacier’s minimum is 40KB.
    14. Objects stored in S3 have a minimum storage duration of 30 days (except for S3 Standard). 
    15. Objects that are archived to Glacier have a minimum 90 days of storage. 
    16. Objects that are deleted, overwritten, or transitioned to a different storage class before the minimum duration will incur the normal usage charge plus a pro-rated request charge for the remainder of the minimum storage duration.
    17. Glacier has a per GB retrieval fee.
    18. You can transition objects from some S3 storage classes to another. Glacier objects can only be transitioned to the Glacier Deep Archive storage class.S3 (standard, intelligent-tiering, standard-IA, and one zone-IA) and Glacier are backed by an SLA. 
16. CloudHub Vs Transit Gateway
    1. Transit Gateway establishes a hub and spoke model between VPCs, Direct Connects, etc
    2. A VPN CloudHub provides a hub and spoke model specifically for VPNs. It helps provide tunnels between your VPN links
    3. A VPN CloudHub is used in a more specific situation than transit gateway and is much simpler to setup for this specific solution
17. ELB Vs ASG
    1. ELB( Elastic Load Balancer ) distributes the traffic load evenly across the available EC2 instances present in the target groups and they ensure that they connect the traffic to appropriate Target Groups.
    2. ELB also monitors the health of EC2 instances and routes the traffic to a healthy instance only. If it finds any unhealthy instance then it stops sending the request to that instance and makes sure that the data request is sent to some other available instance.
    3. Hence the role of ELB is to just distribute the Traffic, check the health of instance and make sure that every request is connected to appropriate target groups.
    4. Auto Scaling increases or decreases the no. of available instances as per the scaling policy mentioned, to manage the instances both in peak and off-peak hours.
    5. It is the work of Auto-Scaling to increase the instance when the Threshold value is exceeded and to remove the instances when they are not being utilized.
    6. This helps to ensure that the application faces low down time.
18. Simple Workflow Service Vs Step Functions
    1. Simple Workflow Service
        1. A web service that makes it easy to coordinate work across distributed application components.
        2. In Amazon SWF, tasks represent invocations of logical steps in applications. Tasks are processed by workers which are programs that interact with Amazon SWF to get tasks, process them, and return their results.
        3. The coordination of tasks involves managing execution dependencies, scheduling, and concurrency in accordance with the logical flow of the application.
    2. Step Functions
        1. A fully managed service that makes it easy to coordinate the components of distributed applications and microservices using visual workflows.
        2. You define state machines that describe your workflow as a series of steps, their relationships, and their inputs and outputs.
        3. State machines contain a number of states, each of which represents an individual step in a workflow diagram. 
        4. States can perform work, make choices, pass parameters, initiate parallel execution, manage timeouts, or terminate your workflow with a success or failure.


