# Snow family & Storage gateway

1. AWS Snow family
    1. Highly secure, portable devices to collect and process data at the edge and migrate data into and out of AWS
    2. Data migration
        1. Snow-cone
            1. Small portable computing anywhere rugged & secure
            2. 8Tbs of usable storage
            3. Must provide own battery/cables
            4. Can be sent to AWS offline or connect it to internet and use AWS dataSync to send data
        2. Snowball edge
            1. Move TBs or PBs
            2. Pay per data transfer job
            3. Provide blocks storage and S3 compatible object storage
            4. Snowball edge storage optimised
                1. 80TB of HDD capacity
            5. Snowball edge compute optimised
                1. 42 TB of HDD capacity
        3. Snowmobile
            1. Transfer exabytes of data (1 EB = 1000 PB = 1000000 TBs)
            2. Each snowmobile has 100 PB of capacity -> use multiple in parallel
            3. High security, temperature controlled, GPS, 24/7 video surveillance
            4. Better than snowball gif you transfer more than 10 PB
        4. Solve problems of
            1. Limited connectivity
            2. Limited bandwidth
            3. High network cost
            4. Shared bandwidth
            5. Connection stability
    3. Edge computing
        1. Process data while its being created on an edge location
        2. Edge locations may have
            1. Limited/ no internet access
            2. Limited / no easy access t computing power
            3. Uses
                1. Prepossess data
                2. Machine learning at edge
                3. Transcoding media streams
        3. Devices
            1. Snow-cone
                1. 2 CPU, 4 GB memory, WIFI or wire access
                2. USB-c power using a cord
            2. Snowball edge - Compute Optimised
                1. 52 vCPUs, 208 GB of RAM
                2. Optional GPU (for video processing and machine learning)
                3. 42 TB of usable storage
            3. Snowball edge - Storage optimised
                1. Upto 40 vCPUs, 80 GB of RAM
                2. Object storagre clustering available
            4. All can run Ec2 instances, AWS lambda functions using AWS IoT Greengrass
            5. Long term deployment options for 1 or 3 years
    4. You need to install snowball client/AWS OpsHub on your servers
    5. AWS OpsHub
        1. To give a GUI for snow devices
    6. Snowball into Glacier
        1. Snowball cannot import to glacier directly
        2. You need to use S3 first in combination with an S3 lifecycle policy
2. Storage gateway
    1. Used in Hybrid cloud
        1. Part of infra in cloud
        2. Part of infra on-premise
    2. Storage gateway bridge between on-premise data and cloud data in S3
    3. Use
        1. Disaster recovery
        2. Backup
        3. Tiered storage
    4. Types
        1. File gateway
            1. Configured S3 buckets are accessible using NFS and SMB protocol
            2. Supports S3 standard, S3 IA, S3 one zone IA
            3. Bucket access using IAM roles for each file gateway
            4. Most recently used data is cached in the file gateway
            5. Can be mounted on many servers
            6. Integrated with Active directory (AD) for user authentication
        2. Volume gateway
            1. Block storage using iSCASI protocol backed by S3
            2. Backed by EBS snapshots which help restore on-premise volumes
            3. Types
                1. Cached volumes
                    1. Low latency access to most recent data
                2. Stored volumes
                    1. Entire dataset is on premise, schedule backups to S3
        3. Tape gateway
            1. Backing using physical tapes
            2. Virtual Tape library (VTL) backed by S3 and glacier
            3. Backup data using tape-based processes and iSCI interface
            4. Works with leaded software vendors
    5. Gateways are generally installed on-premise
    6. If you don’t have capacity at on-premise use Storage gateway-Hardware appliance
        1. You need on-premise virtualisation
        2. Else you can use a get a Storage gateway hardware appliance
        3. Helpful for daily NFS backups daily in small database
3. AWS FSx for Windows
    1. EFS is a shared POSIX system for linux systems (Cannot be used for windows)
    2. FSx is a fully managed windows file system share drive
    3. Supports SMB protocol & Windows NTFS
    4. Microsoft Active directly integration, ACLs, user quotas
    5. Built on SSD
    6. Can be access from on-premise infrastructure
    7. Can be configured to be Multi AZ
    8. Data is backed up daily to S3
4. AWS FSx for lustre
    1. Name is derived from Linux + cluster
    2. Lustre is a type of parallel distributed file system for large scale computing
    3. Use
        1. Machine learning
        2. High performance computing (HPC)
        3. Video processing
        4. Financial modelling
    4. Scales upto 100s of GB/s
    5. Seamless integration with S3
        1. Can read S3 as a file system through FSx
        2. Can write the output of computation back to S3 through Fsx
    6. Can be used from on-premise servers
    7. Lives in only 1 AZ
5. FSx deployment options
    1. Scratch file system
        1. Temporary storage
        2. Data is not replicated
        3. High bust -> 6x faster
        4. Use
            1. Short term processing of data
    2. Persistent file system
        1. Long term storage
        2. Data is replicated within same AZ
        3. Replace failed files within minutes
        4. Use
            1. Long term processing
            2. Sensitive data
6. AWS Transfer family
    1. A fully managed service service for file transfers into and out of Amazon S3 or EFS using FTP protocol
    2. Supported protocols
        1. AWS Transfer for FTP -> File Transfer Protocol
        2. AWS Transfer for FTPS -> File Transfer Protocol over SSL
        3. AWS Transfer for SFTP -> Secure File Transfer Protocol
    3. Managed infra, scalable, reliable, Multi AZ
    4. Pay per provisioned endpoint per hour+data transfer in GB
    5. Store and manage users credentials within the service
    6. Integrate with existing authentication system
    7. Usage
        1. Sharing files, public datasets
7. All storage comparisons
    1. S3
        1. Object Storage
        2. Serverless
    2. Glacier
        1. Object Archival
    3. EFS
        1. Network file system for linux instances
        2. Can be shared with multiple instances across AZ
    4. FSx for windows
        1. Network file system for windows servers
    5. FSx for lustre
        1. High performance computing for linux file systems
    6. EBS volumes
        1. Network storage for one EC2 instance at a time
        2. Bound to AZ
        3. To change AZ move snapshot
    7. Instance storage
        1. Physical storage for EC2 instance
        2. If Ec2 instance goes down Data is lost
    8. Storage gateway
        1. Transporting files from On-premise to AWS
        2. File gateway
        3. Volume gateway
        4. Tape gateway
    9. Snowball
        1. To move large amount of data to the cloud, physically 
    10. Database
        1. For specific workloads, usually with indexing and query
        2. RDS
        3. DynamoDB
