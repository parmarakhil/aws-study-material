# Disaster recovery

1. Types
    1. On-premise => On-premise
        1. Traditional DR 
        2. Very expensive
    2. On-premise => AWS cloud
        1. Hybrid recovery
    3. AWS cloud region A => region B
2. RPO -> Recovery Point Objective
    1. How back in time can you recover
    2. Data is lost is Time between RPO and Disaster 
3. RTO -> Recovery Time objective
    1. Downtime between Disaster and RTO
4. Strategies
    1. Backup and restore
        1. High RPO
        2. Less expensive
    2. Pilot light
        1. A small version of the app is always running in the cloud
        2. Useful for critical core - pilot light
        3. Similar to backup and restore
        4. Faster than backups and restore as critical systems are already up
        5. Expensive than Backup & restore
        6. Lower RPO
        7. Lower RTO
    3. Warm standby
        1. Full system is up and running at minimum size
        2. Upon disaster, we can scale to production load
    4. Hot site/Multi site approach
        1. Very low RTO - minutes or seconds
        2. Very expensive
        3. Full productions Cale is running AWS and on-premise
        4. Active-active setup
5. Database Migration service
    1. Quickly and securely. Migrate databases to AWS, resilient , self healing
    2. The source database remains available during migration
    3. Supports
        1. Homogenous migration eg MySQL to MySQL
        2. Heterogeneous migration eg MsSQL to Aurora
    4. Continuous Data replication using CDC
    5. You need to create EC2 instance to perform replication tasks
    6. Schema Conversion tool (SCT)
        1. Convert your Database schema from one engine to another
        2. Example OLTP -> Oracle to MySQL
        3. Ecamle OLAP -> Oracle to Redshift
        4. You don’t need to use it if both source and target engines are same
6. On-premise Strategy with AWS
    1. Ability to download Amazon Linux 2 AMI as VM
    2. VM import/export
        1. Migrate existing application into Ec2
        2. Create a DR repository strategy for On-premise vm
        3. Can export back the VM from EC2 to on-premise
    3. AWS Application discovery service
        1. Gather info about your on-premise server to plan a migration
        2. Server utilisation and dependency mappings
        3. Track with AWS migration hub
    4. AWS Database migration service (DMS)
    5. AWS Server migration service (SMS)
        1. Incremental replication of on-premise live servers to AWS
7. AWS Datasync
    1. Move large amount of data from on-premise to AWS
    2. Can synchronise to S3, EFS, FSx for windows
    3. Move data from your NAS or file system via NFS or SMB
    4. Replication tasks can be scheduled hourly, daily, weekly
    5. Leverage the data sync agent to connect to you systems
    6. Can setup bandwidth
    7. Can sync EFS between two regions
8. AWS Backup
    1. Fully managed backup service
    2. Centrally mange and automate backups across AWS services
    3. No need to create custom scripts and manual processes
    4. Supports Cross region backups
    5. Supports cross account backups
    6. Supports Point in time recovery services
    7. On-demand and scheduled backups
    8. Tag based backup policies
    9. Create polices known as backup plans
        1. Frequency
        2. Backup Window
        3. Transition to clod storage
        4. Retention period
    10. 
9. Summary
    1. Over the internet / Site-to-Site VPN
        1. Immediate to setup
        2. Takes long time
    2. Over direct connect
        1. Long for one time setup (1 month)
        2. 10x faster than Site-to-Site
    3. Over Snowball
        1. Will take about 1 week for end-to end transfer
        2. Can be combined with DMS
    4. For on-going replication/transfers
        1. Site-to-Site VPN
        2. DX with DMS
        3. DataSync

