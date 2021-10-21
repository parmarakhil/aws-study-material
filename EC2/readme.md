# EC2

1. EC2 is Elastic compute cloud
2. It is IAAS
3. It consists the capability of
    1. Renting virtual machine (EC2)
    2. Storing data on virtual drives (EBS)
    3. Distributing loas across machines (ELB)
    4. Scaling the services using an auto scaling group (ASG)
4. Sizing & configuring options
    1. OS —> linux, windows, Mac OS
    2. CPU
    3. RAM
    4. Storage
        1. Network attached —> EBS & EFS
        2. Hardware (EC2 instance store)
    5. Network card
    6. Security group
    7. EC2 user data —> bootstrap script , configure at first launch
        1. Runs with root user
5. Types
    1. General purpose
        1. Great for diversity of workloads
        2. Balance between
            1. Compute
            2. Memory
            3. Networking
    2. Compute optimised
        1. Great for compute intensive tasks that require high performance tasks
            1. Batch processing
            2. Media transcoding
            3. High performance web servers
            4. High performance coding
            5. Gaming
    3. Memory optimised
        1. Fast performance for workloads that process large data sets in memory
            1. High performance databases
            2. Distributed web scale cache stores
            3. In-memory database for BI
    4. Accelerated computing
    5. Storage Optimised
        1. Create for storage intensive tasks that require highm sequential read and write access to large data sets on local storage
            1. High frequency online transaction processing (OLTP) systems
            2. Relational & NO SQL databases
            3. Cache for in-memory databases
            4. Data warehousing applications
            5. Distributed file systems
    6. Instance Features
    7. Measuring Instance performance
6. Security Groups
    1. Security groups are fundamental of network security in AWS
    2. They control how traffic is allowed in & out of EC2 instances
    3. Can only contain allow rules
    4. Security group rules can reference by IP or by Security group
    5. Security groups are acting as firewall on EC2 instances
    6. They regulate
        1. Access to ports
        2. Authorised IP ranges I.e IPv4 & IPv6 ranges
        3. Control inbound and outbound network
    7. Security groups can be attached to multiple instances
    8. They are locked down to a region/VPC combination
    9. Does live outside the EC2
    10. If your application is not accessible I.e timeout error then it is security group issue
    11. If application gives a connection refused error the issue is with application or it is not launched
    12. By default all inbound traffic is blocked and all outbound traffic is allowed
    13. We can refernece other security groups also
    14. Port 22 -> SSH  (Secure shell) - log into a linux instance
    15. Port 21 -> FTP (File Transfer Protocol) - upload files into a file share
    16. Port 22 -> SFTP (Secure File Transfer Protocol) - upload files using SSH
    17. Port 80 -> HTTP - access unsecured websites
    18. Port 443 -> HTTPS - access secured websites
    19. Port 3389 -> RDP (Remote Desktop Protocol) - log into a windows instance
7. To connect to EC2 instance from your machine
    1. You need they key-pair file (.pem) file
    2. If you get private file error, run “chmod 0400 keypairfile.pem”
    3. Ways
        1. SSH for linux, Mac, Windows 10+
        2. Putty for windows 10-
        3. EC2 Instance connect (Uses SSSH internally)
8. Instance roles (IAM roles)
    1. Never add your access key ID and secret key on EC2 instance
    2. Use roles to give EC2 instance access to different EC2 instances
9. EC2 types
    1. On demand instance —> short workload, predicatable pricing
        1. Pay for what you use
            1. For linux —> billing is per second after first minute
            2. Other OS —> billing per hour
        2. Has highest cost 
        3. No upfront payment
        4. No long term commitment
        5. Best for short workloads that are unpredictable, un-interrupted workloads
    2. Reserved —> Minimum 1 year
        1. Reserved instances -> long workloads
            1. Upto 75% discount compared to On-demand
            2. Reservation period 1 to 3 year
            3. Purchasing option
                1. No upfront
                2. Partial upfront
                3. Full upfront
            4. The more duration and early payment , the more discount
            5. Reserve a specific instance type
            6. Best for steady-state usage applications eg database
        2. Convertible reserved instances -> long workload with flexible instances
            1. Can change EC2 instance type
            2. Upto 54% discount
        3. Scheduled reserved instances -> example every Sunday 10:am
            1. Launch within specific time period
    3. Spot instances —> short workloads, cheap, can loose instances I.e. less reliable
        1. Highest discount. Upto 90%
        2. You can loose instance at any point of time
        3. Most cost efficient
        4. Best for workloads that are resilient to failure
            1. Batch jobs
            2. Data analysis
            3. Image processing
            4. Distributed workloads
            5. Workloads that. Have flexible start and end time
        5. Worst for critical workloads and database
        6. If the current spot price > your max price, you can choose to stop or terminate the instance within a grace period of 2 minutes
        7. Other strategy
            1. Spot block
                1. Block spot instance during a specified time frame (1 to 6 hours)  without interruption
        8. You can make one time request or persistent requests to get spot instances
            1. Incase of persistent requests you will get another instance after current one is terminated once the price is under your max prices
        9. You can only cancel spot instances requests that are open, active or disabled
        10. Cancelling spot request does not terminate instances
        11. Spot fleets
            1. Spot fleet = set of spot instances + Ondemand instances (Optional)
            2. The spot fleet will try to meet. Target capacity with price constraint
            3. Strategies to allocate spot instance
                1. LowestPrice -> from the pool with lowest prices
                    1. Cost optimisation
                    2. Short workload
                2. Diversified -> distributed across all pools
                    1. Great for availability
                    2. Long workloads
                3. CapacityOptimised > pool with optimal capacity. For the number of instances
            4. Spot fleet allow us to automatically request spot instances with lowest price
    4. Dedicated hosts —> book an entire physical server, control instance placement
        1. An EC2 dedicated host is a physical server with EC2 instance  capacity fully dedicated to your use.
        2. Dedicated hosts can help you address compliance requirements
        3. They help reduce cost by allowing you to use your existing server-bound software licenses
        4. Allocated for 3 period reservation
        5. More expensive
        6. Useful for complicated licensing model eg BYOL Bring Your Own license
        7. Used in cases where strong regulatory or compliance is required
    5. Dedicated instances
        1. Instances running on hardware that is dedicated to you
        2. May share hardware with other instances in same account
        3. No control over how instance is placed (can move hardware stop/start)

