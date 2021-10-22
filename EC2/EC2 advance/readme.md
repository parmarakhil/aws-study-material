# EC2 advance

1. Private IP , Public IP, Elastic IP
2. Types of IP
    1. IPv4 -> common format used online
    2. IPv6 -> new and solves problems of IOT
3. Public IP’s are accessible over internet, must be unique across whole web, can be geo located easily
4. Private IP’s are accessible within private network, must be unique across private network
    1. Two private network (private companies) can have same IP’s
    2. Machines connect to internet using internet gateway (a proxy)
    3. Only a specified range of IP’s can be used as private IP
5. Elastic IP
    1. When we start/stop instance public IP change
    2. If we need same public IP we can attach an elastic IP
    3. You can have 5 elastic IP in your account
    4. If you create an elastic IP and don’t use it, you will be charged some money as penalty
6. By default EC2 comes with private & public IP
7. Placement groups
    1. Used when you want to control the placement of EC2 instances
    2. Strategies
        1. Cluster
            1. Cluster instances into a low latency group in a single AZ
            2. Pros
                1. Great network 10Gbps bandwidth between instances
            3. Cons
                1. If rack fails, all instances fails at same time
            4. Uses
                1. Big data job
                2. Application that needs extremely low latency and high network throughput
        2. Spread
            1. Spreads instances across underlying hardware
            2. Max 7 instances per AZ
            3. For critical applications
            4. Pros
                1. Can span across multiple AZ
                2. Reduced risk is simulated failures
                3. EC2 instances are on different physical hardware
            5. Cons
                1. Limited to 7 instances per AZ per placement group
            6. Use
                1. Applications that need high availability
                2. Critical applications where instances are isolated to reduce total failure
        3. Partition 
            1. Spreads instances across many different partitions which reply on different sets of racks within an AZ
            2. Scales to 100s of EC2 instances per group
            3. Used in Hadoop, Casandra , Kafka I.e distributed applications
            4. Each partitions represent a rack
            5. Upto 7 partition per AZ
            6. Can spread across multiple AZ in same region
            7. Each partition is isolated from other
            8. EC2 instances get access  to the partition information as metadata
8. Elastic network interfaces
    1. Logical component in a VPC that represent a virtual network card
    2. Network card provide network connectivity 
    3. The. ENI can have
        1. Private primary IPv4
        2. One or more secondary IPv4
        3. One public IPv4
        4. One or more security group
        5. A MAC address
    4. You can create ENI independently and attach them on fly on EC2 instances for failure
    5. Bound to a specific AZ
9. Hibernate
    1. If we stop Ec2 the data on disk (EBS) is kept intact in the next start
    2. If we terminate EC2 any EBS volumes (root) also setup to be destroyed is lost
    3. On first start the OS boots & EC2 runs user data script is run
    4. Subsequent starts will only boot OS
    5. Then your application starts, caches gets warmed up and this can take time
    6. Using EC2 hibernate
        1. In memory (RAM) state is preserved
        2. The instance boot is much faster because OS is not stopped/restarted
        3. Under the hood, the RAM state is written to a file in root EBS 
        4. The root EBS must be encrypted
    7. Use cases
        1. Keep long running processing
        2. Services that take time to initialise
    8. Instance RAM size must be less than 150GB
    9. Not supported for bare metal instances
    10. Root volume must be EBS, encrypted, not instance store and should be large
    11. Available for on demand and reserved instances
    12. An instance cannot be hibernated more than 60 days
10. EC2 Nitro
    1. Underlying platform for the next generation of EC2 instances
    2. New virtualisation technology
    3. Allows better networking options (enhanced networking, HPC, IPv6)
    4. Allows higher Speed EBS (Nitro is necessary for 64,000 EBS IOPS - max 32,000 on non-Nitro )
    5. Better underlying security
    6. Instance Types
        1. Virtualised
        2. Bare metal
11. vCPU
    1. Multiple threads can run on one CPU
    2. Each thread is represented as a virtual CPU (vCPU)
    3. 2 threads per CPU
    4. EC2 instances come with a combination of RAM and vCPU
    5. Used where we want too optimise number of CPUs and no of threads per core, this can be useful for licensing cost
    6. Only specified during instance launch
12. Capacity reservation
    1. Capacity reservations ensures you have EC2 capacity when needed
    2. Manual or planned end-date for the reservation
    3. No need for 10r 3 year commitment
    4. Capacity access is immediate so you get billed as soon as it starts
    5. Combine with reserve instances and savings plan to do cost saving
    6. 
