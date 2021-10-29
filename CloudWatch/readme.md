# Cloudwatch

1. Cloudwatch provides metrics for every services in AWS
2. Metric is a variable to monitor eg CPUUtilisation
3. Metric belong to namespaces
4. Dimension is an attribute of a metric
5. Upto 10 dimensions per metric
6. Metrics have timestamps
7. Can create CloudWatch dashboards of metrics
8. EC2 detailed monitoring
    1. By Default EC2 instance metrics have metrics every 5 minutes
    2. With detailed monitoring you can get data every 1 minute
    3. This can enable you to react faster to metrics
    4. Note: EC2 memory usage is not pushed by default, the application has to push it to cloud watch as a custom metric
9. Cloudwatch custom metrics
    1. You can send custom metrics
    2. Eg : RAM usage, disk space
    3. Use API call PutMetricData
    4. Ability to use dimensions to segment metrics
        1. Instance.Id
        2. Environment.name
    5. Metric resolution (StorageResolution API parameter - two possible value)
        1. Standard - 1 minute (60 seconds)
        2. High Resolution : 1/5/10/30 seconds - Higher cost
    6. Accepts metric data points two weeks int he past and two hours in future hence EC2 instance time should be set correctly
10. Cloudwatch dashboards
    1. Dashboards are global
    2. Can include graphs from different AWS accounts and regions
    3. You can change the time zone & time range of the dashboards
    4. You can setup automatic refresh
    5. Dashboards can be shared with people who don’t have AWS account 
    6. You have 3 dashboards for free post that you pay $3/dashboard/month
11. AWS Cloudwatch logs
    1. Applications can send logs using SDK
    2. Can also collect log from
        1. Elastic Beanstalk
        2. ECS
        3. Lambda
        4. VPC
        5. Cloudtrail
        6. API gateway
        7. Cloudwatch log agents
        8. Route53
    3. Logs can go to
        1. S3 for archival
    4. Logs storage architecture
        1. Log groups
            1. Arbitrary name, usually representing an application
        2. Log stream
            1. Instances within application / log files/ containers
    5. Can define log expiration policy eg never expire, 30 days
    6. Using CLIw e can track cloud watch logs
    7. To send logs you should have IAM permissions
    8. Logs can be encrypted using KMS
    9. Filter & Insights
        1. Can use filter expressions
            1. Eg: find a specific IP 
            2. Metric filters can be used to trigger alarms
        2. Cloudwatch logs insights can be used to query logs and add queries to cloud watch dashboards
12. Cloudwatch logs for EC2
    1. By default no logs from your EC2 machine will go to CloudWatch
    2. You need to run Cloudwatch agent on EC2 to push logs to cloudwatch
    3. Ec2 instance must have IAM role to send logs
    4. Cloudwatch log agent can be setup on on-premise server
    5. Cloudwatch agent & Unified agent
        1. Run on virtual server
        2. Cloudwatch logs agent
            1. Old version of the agent
            2. Can only send to cloudwatch logs
        3. Cloudwatch Unified agent
            1. Collect additional system level metrics such as RAM, processes etc
            2. Collect logs to send to CLloudwatch logs
            3. Centralised configuration using SSM parameter store
            4. CPU, Disk metrics, RAM, net stat, Processes, Swap space
13. Cloudwatch Alarm
    1. Used to trigger notification for any metric
    2. Various options 
        1. Sampling
        2. %
        3. Max
        4. Min
    3. Alarm states
        1. OK
        2. Insufficient data
        3. Alaram
    4. Period
        1. Length of time in seconds to evaluate the metric
        2. High resolution custom metrics: 10sec, 30 sec or multiple of 60 sec
    5. Targets
        1. Actions on EC2 eg Stop, terminate, reboot
        2. Trigger Auto scaling action
        3. Send notification to SNS
    6. For Ec2 instance recovery
        1. Cloudwatch alarm will check status and its action would be to recover
        2. Status check
            1. Instance status = check the EC2 VM
            2. System status = check the underlying hardware
        3. Recovery
            1. You will get same Private, public, Elastic IP, metadata, placement group
    7. Alarms can be created based on cloudwatch logs metric filters
14. Cloudwatch events
    1. Intercept events from AWS services/sources using event patterns
        1. Can intercept any API call with cloud trail integration
    2. Schedule to Cron
        1. Create event every X time
    3. A json payload is created from event and then passed to target
