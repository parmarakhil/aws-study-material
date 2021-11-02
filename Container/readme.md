# Containers


1. Apps are packaged in containers that can be run on a ny OS
2. Apps run the same regardless of where they’re run
3. Docker images are stored in docker repositories
    1. Public - Docker Hub
    2. Private - Amazon ECR - Elastic Container Repository
4. Amazons container platforms
    1. ECS
    2. Fargate
    3. EKS
5. ECS
    1. Elastic Container Service
    2. You must provision & maintain the infrastructure like EC2 instances
    3. AWS takes care to starting/stopping containers
    4. Has integrations with ALB
    5. Launch types
        1. Amazon EC2 launch type for ECS
    6. ECS Agent is configured and kept running
    7. ECS tasks are then started on the server
6. Fargate
    1. You don’t need to provision Infrastructure I.e EC2 instance
    2. Serverless
    3. AWS run containers based on CPU or RAM we need
    4. ENI is used to access tasks
        1. ENI binds tasks to IPs
        2. 1 task = 1 ENI
7. IAM Roles for ECS tasks
    1. EC2 instance profile
        1. Used by ECS agent
        2. Makes APIc alls to ECS services
        3. Send container logs to cloud watch logs
        4. Pull docker image from ECR
        5. Reference sensitive data in secrets manager
    2. ECS tasks role
        1. Attached to task
        2. Each task has specific role
        3. Use different roles for different ECS services
        4. Task role is defined in task definiton
8. ECS has integration with EFS to be used as a volumes (both EC2 instance and Margate task)
9. EFS is mount to tasks hence tasks launched in any AZ share same data using EFS volumes
10. ECS services & Tasks
    1. 1 service can run many tasks
    2. ALB can be attached to tasks
    3. 1 ECS cluster can run many services
    4. Load balancing for EC2 launch type
        1. Each tasks gets random port
        2. We get dynamic port mapping
            1. ALB finds the right port on your EC2 instances
            2. Reduces trouble of port mapping
        3. You must allow on the EC2 instance security group any port from the ALB security group
    5. Load balancing for Margate
        1. ENI has unique IP but same port
        2. ALB talk to same port
        3. Each task has unique IP
        4. You must allow the ENI’s SG allow task port on ALB
11. ECS tasks invoked by event bridge
    1. Event run ECS task
    2. Can be used with cloudfront also
    3. ECS task role should have necessary permission
12. ECS Scaling
    1. Based on Service CPU usage
        1. Triggers cloud watch alarm using metric 
        2. Increase ASG  (EC2 instance)using Scale ECS capacity providers
    2. Based on SQS queue
        1. Based on cloud watch metric (queue length) trigger alarm
13. ECS Rolling updates
    1. When updating service from v1 to v2
    2. Control the way v2 will be launched
    3. Create new tasks with v2 and terminate v1 tasks in batches keeping in mind min capacity required
14. ECR
    1. Elastic container Registry
    2. Store, manage and deploy container in ECS
    3. Pay for what you use
    4. Fully integrated with ECS & IAM for security
    5. Backed by S3
    6. Supports image vulnerability scanning, version, tag, image lifecycle
15. EKS
    1. Elastic Kubernetes Service
    2. Way to launch managed Kubernetes clusters on AWS
    3. Alternative to ECS
    4. Launch modes
        1. EC2 
        2. Margate
    5. Use case
        1. If company is already using Kubernetes
    6. It is cloud agnostic - can be used in any cloud
    7. EKS node has many EKS pods
    8. Can be integrated with ALB
