# Elastic Block Storage Volumes
- Elastic Block Storage (EBS) is a network drive you can atach to your instance while they run
- It allows your instances to persist data
- It's a network drive (ie not a physical drive)
    - It uses the netwrok to communicate the instance, which means there might be a bit of latency
    - It can be detached from an EC2 instance and attached to another one quickly
- It's locked to an AZ
    - Cannot attach EBS volume to an EC2 in another AZ
    - To move a volume across to a different AZ you first need to snapshot it
- Provision Capacity (size in GBs and IOPS)
    - You get billed for provisional capacity (not for how much you use)
    - Can increase the capacity of drive over time

## EBS Volume Types
- GP2(SSD) - General purpose SSD volume that balances price and performance for a wide variety of workloads
- IO1(SSD) - Highest performace SSD volume for mission-critical low latency or high throughput workloads
- ST1(HDD) - Low cost HDD volume designed for frequently accessed, throughput intensive workloads
- SC1(HDD) - Lowest cost HDD volume designed for less frequently accessed workloads
- Only GP2 and IO1 can be used as boot volumes
- EBS Volumes are characterised by Size, Throughput, IOPS (I/O Ops per Sec)

## EBS Volume Types Use cases
- GP2
    - Recommended for most workload
    - System boot volumes
    - Virtual desktops
    - Low-latency interactive apps
    - Development and test environments
    - 1 GiB - 16 TiB
    - Small gp2 volumes can burst IOPS to 3000
    - Max IOPS is 16000
    - 3 IOPS per GB, means at 5334 GB we are at the max IOPS
- IO1
    - Critical business applications that require sustained IOPS performance, or more than 16000 IOPS per volume (gp2 limit)
    - Large database workloads eg MongoDB, Cassandra, Microsoft SQL Server, MySQL, PostgresSQL, Oracle,etc
    - 4GiB - 16TiB
    - IOPS is provisioned (PIOPS) - MIN 100 - MAX - 64000 (Nitro instance) else MAX 32000 (other instances)
    - Max ration of provisioned IOPS to requested volume size (in GiB) is 50:1
- ST1
    - Streaming workloads required consistent, fast throughput at a low price
    - Big data, Data warehouse, Log processing, Apache Kafka
    - Cannot be a boot volume
    - 500 GiB - 16 TiB
    - Max IOPS is 500
    - Max throughtput is 500 MiB/s - can burst
- SC1
    - Throughput-oriented storage for volumes of data that is infrequently accessed
    - Scenarios where the lowest storage cost is important
    - Cannot be a boot volume
    - 500 GiB - 16 TiB
    - Max IOPS is 250
    - Max throughtput is 250 MiB/s - can burst

## Instance store
- Some instances do not come with Root EBS volumes
- Instead they come with "Instance Store"(=ephemeral storage)
- Instance store is physically attached to the machine (EBS is a network drive)
- Pros
    - Better I/O performance
    - Good for buffer / cache / scratch data / temporary content
    - Data survives reboot
    - Upto 7.5 TiB
- Cons
    - On stop or termination, the instance store is lost
    - You can't resize the instance store
    - Backups must be operated by the user
    - Risk of data loss if hardware fails

# Elastic File System (EFS)
- Managed NFS that can be mounted on many EC2 across AZs
- Highly available, scalable, expensive (3 x gp2), pay per use
- Use case: Content management, web serving, data sharing, wordpress
- Uses NFSv4.1 protocol
- Uses security group to control access to EFS
- Compatible only with Linux AMIs (not windows)
- Encryption ar rest using KMS 
- Filesystem scales automatically, pay per use, no capacity planning
- 1000s of concurrent NFS clients, 10 GB+/s throughput
- Grow to Petabytpe-scale network file system, automatically
- Performance mode (set at EFS creation time)
    - General purpose (default): latency-sensitive use cases (web server, CMS, etc)
    - Max I/O - higher latency, throughput, highly parallel (big data, media processing)
- Storage Tier (lifecycle management feature - move file after N days)
    - Standard: for frequently accessed files
    - Infrequest access (EFS-IA): cost to retrieve files, lower prioce to store

