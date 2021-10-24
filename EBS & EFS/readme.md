# EBS & EFS

1. EBS 
    1. An Elastic block store volume is a network drive you can attach to your instances while they run
    2. It allows your instances to persists data even after their termination
    3. They can be mounted to only one instance at a time, however for some EBS you can mount instances with multiple EBS volumes
    4. They are bound to specific AZ
    5. Think of them as network USB stick
    6. It uses network to communicate with instance so a latency can be expected 
    7. To move it across AZ’s you first need to create a snapshot
    8. Have a provisioned capacity (size in GBs and IOPS)
    9. You can increase capacity over time
    10. They can be left unattached
    11. Delete on termination attribute helps delete EBS volume when EC2 is terminated
    12. By default root EBS volume is deleted on EC2 termination and others are not
2. EBS Snapshots
    1. Used to make a backup (snapshot) of your EBS volume at a point in time
    2. Not necessary to detach volume to do snapshot but recommended
    3. Can copy snapshot across AZ or Regions
3. AMI
    1. AMI is Amazon Machine Image
    2. AMI are a customisation of an EC2 instance
    3. You can configure your own softwares, OS, configurations etc
    4. Faster boot time as all your softwares are pre packaged
    5. AMI are built for a specific region and can be copied across regions
    6. You can launch EC2 instances from
        1. Public AMI -> AWS provided
        2. Your own AMI -> you make and maintain them yourself
        3. An AWS marketplace AMI -> An AMI someone else made
    7. Steps
        1. Start EC2 instance
        2. Customise it
        3. Stop the instance for data integrity
        4. Build an AMI - also includes EBS snapshots
        5. Launch instances from the AMIs
4. EC2 Instance store
    1. EBS volumes are network drives with good but limited performance
    2. If you need a high performance hardware disk, use EC2 instance store
    3. It has physical connection with EC2 instance
    4. Better I/O performance
    5. If stop/ terminate the EC2 instance the data will be lost this is why it is ephemeral storage
    6. Good for buffer/cache/temporary content
    7. Risk of data loss if hardware fails
    8. Backups and replication is your responsibility
5. EBS volume types
    1. Gp2/gp3 SSD
        1. General purpose SSD volumes
        2. Balances price and performance
        3. Gp2
            1. Cost effective, low latency
            2. System boot volumes, virtual desktops, dev and test environments
            3. 1 GB to 16 TB
            4. Small gp2 volume can burst IOPS to 3000
            5. Size of volume and IOPS are linked, max IOPS is 16,000
            6. 3 IOPS per GB, means at 5334 GB we are at. Max IOPS
        4. Gp3
            1. Baseline of 3000 IOPS and throughput of 125 MB/s
            2. Can increase IOPS upto 16,000 and throughput up to 1000 MB/s independently 
    2. Io1/io2 SSD
        1. Used for multi attach
        2. Can attach same EBS volume to multiple EC2 instances in same AZ
        3. Each EC2 instance has full read & write permissions to the volume
        4. Muti attach uses
            1. Achieve higher application availability in cluster linux applications
            2. Applications must manage concurrent write operations
        5. Highest performance SSD
        6. For mission critical workload
        7. Low latency or high throughput workloads
        8. Applications that need more than 16000 IOPS
        9. Great for databases workloads
        10. 4Gb to 16TB
        11. Max IOPS for 64000 for nitro EC2 and 32000 for other
        12. Can increase PIOPS independently from storage size
        13. Io2 has more durability and more IOPS per GB at same price as io1
        14. Io2 block express give sub millisecond latency, 256000 Max IOPS with IOPS:GB ratio of 1000:1
        15. Supports EBS multi attach
    3. St1 HDD
        1. Frequent access throughput intensive workloads
        2. Big data, Dara warehouses, log processing
        3. Max throughput 500MB/s max IOPS 300
    4. Sc1 HDD
        1. Lowest cost HDD
        2. Max throughput 250MB/s max IOPS 250
        3. Designed for less frequently accessed workloads
    5. EBS volumes are characterised in size | throughput | IOPS
    6. Only gp2/gp3 and io1/io2 can be used as boot volumes
6. EBS encryptions
    1. When you create an encrypted EBS volume
        1. Data at rest is encrypted inside the volume
        2. All data in flight moving between instance and volume is encrypted
        3. All snapshots are encrypted
        4. All volumes created from snapshot are encrypted
    2. Encryption and decryption are handled transparently
    3. Encryption has minimal impact on latency
    4. EBS encryption leverages keys from KMS (AES-256)
    5. Copying an unencrypted snapshot allows encryption
    6. Snapshots of encrypted volumes are encrypted
    7. Create encrypted volume from unencrypted volume
        1. Create EBS snapshot of the volume
        2. Encrypt the snapshot using copy Function
        3. Create new abs volume from the snapshot
        4. Now attach new volume to instance
7. EBS RAID
    1. EBS is redundant storage 
    2. To increase IOPS to eg 1000000 IOPS, to mirror EBS volumes you need to mount volumes in parallel in RAID settings
    3. RAID is possible in linux and windows OS
    4. RAID options 
        1. RAID 0
            1. Used to Increase performance
            2. 1 logical volumes Backed by 2 or more EBS volume
            3. We write to either one of the attached volumes
            4. Combining 2 or more volumes and getting the total desired disk space &. I/O, thus we can create a very big disk with lot of IOPS
            5. But one disk fails, all data is failed
            6. Increase performance at the cost of risk
            7. Use
                1. An application that needs huge IOPS and doesn’t need fault tolerance
                2. A database having replication already built-in
            8. Eg: two 100 GB EBS io1 volumes with 4000 PIOPS will give 200 GB EBS with 8000 PIOPS and 1000 throughput
        2. RAID 1
            1. Used to increase fault tolerance
            2. Same as RAID 0 but this time we write in all the attached volumes
            3. Also called mirroring a volume to another
            4. If 1 disk fails we still have things working
            5. We have to send data to 2 EBS volumes at at the same time therefore 2X network usage
            6. Use
                1. Application that need increase volume fault tolerance or where you need to service disks
            7. Eg: two 100 GB EBS io1 volumes with 4000 PIOPS will give 100 GB EBS with 4000 PIOPS and 500 throughput
            8. Hence no performance improvement but we achieve fault tolerance
        3. RAID 5 (not recommended for EBS)
        4. RAID 6 (not recommended for EBS)
8. EFS
    1. Elastic File system
    2. Managed network  file system which can be mounted to many EC2 instances across AZ (Multi AZ)
    3. Highly available, scalable, expensive, pay per use
    4. You can attach security group to EFS
    5. Use case
        1. Content management
        2. Web serving
        3. Data sharing
    6. Uses NFSv4.1 protocol
    7. Only works with linux based AMI and not windows
    8. Encryption at rest using KMS
    9. No capacity planning is required
    10. EFS Scale
        1. 1000s of concurrent NFS client, 10+GB/s throughput
        2. Grow to petabyte scale automatically
    11. Performance mode 
        1. It is set at EFS creation time
        2. General purpose -> default
            1. Latency. Sensitive use cases eg web server
        3. Max I/O 
            1. Higher latency, throughout, high parallel processing
    12. Throughput mode
        1. Bursting (1Tb = 50 MB/s + burst upto 100MB/s)
        2. Provisioned -> set your throughput regardless of storage size
            1. Eg 1Gb/s for 1Tb storage
    13. Storagre tiers
        1. Life cycle management feature -> move file after N days
        2. Standard -> for frequently accessed files
        3. Infrequent access -> cost to retrieve files, lower price to store
9. EBS vs EFS
    1. EBS 
        1. Can be attached to one instance at a time
        2. Are locked at AZ level
        3. Gp2: IO increase if disk size increase
        4. Io1 -> can increase IO independently
        5. root EBS volumes of instances gets terminated by default if the EC2 instance gets terminated
        6. To migrate EBS volume across AZ
            1. Take a snapshot and Restore snapshot in another AZ
            2. EBS backups use IO so do it when application is handling low traffic
        7. Have to provision capacity before hand
    2. EFS
        1. Can be attached to 100s of instances across AZ
        2. Can be used only with linux based AMI EC2
        3. Share website files
        4. Higher price
        5. Can leverage EFS-IA for cost savings using life cycle policies
        6. Bill for what you use
