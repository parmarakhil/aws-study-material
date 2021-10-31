# Beanstalk

1. To launch application faster
    1. Ec2 instances
        1. Use a golden AMI which has your applications, OS, dependencies before hand and then launch instances from this AMI
        2. Bootstrap using user data -> for dynamic configuration
        3. Hybrid: Mix golden AMI and user data (used in Elastic beanstalk)
    2. RDS database
        1. Restore from snapshot
    3. EBS volumes
        1. Restore from snapshot
2. Elastic Beanstalk
    1. Solves problems of
        1. Managing infra
        2. Deploying code
        3. Configuring database, load balancers etc
        4. Scaling concerns
    2. Beanstalk give a developer centric view of deploying an application on AWS
    3. It is a managed service that handles provisioning, load balancing, scaling, application health etc
    4. The application code is the responsibility of the developer
    5. You all have full control over the configuration
    6. You pay for underlying instances
    7. Components
        1. Application
            1. Collection of beanstalk components
        2. Application version
            1. An iteration of application code
        3. Environment
            1. Collection of AWS resources running an application version
            2. You can run only one version at a time
            3. Tiers —> Server environment their & worker environment tier
            4. Can have multiple environment eg dev, test, prod
    8. Supported many programming languages and other technologies platform
    9. If your required platform is not available your can create own platform
    10. Web tier vs worker tier
        1. Web —> Uses client accessed resources eg ALB
        2. Worker -> uses backend resources eg SQS queue
            1. Scale based on number of SQS messages
            2. Can push messages to SQS queue from another web server tier
    11. You need to choose platform

