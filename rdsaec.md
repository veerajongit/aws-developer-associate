# Relational Database Service
- Uses SQL as a query language
- eg. Postgres, MySQL, MariaDB, Oracle, Microsoft SQL Server and Aurora (AWS Proprietary database)
- Advantages of using RDS over deploying DB on EC2 
    - RDS is a managed service
        - Provisioning, OS patching
        - Continuous backup and db restore option
        - Monitoring dashboards
        - Read replicas for improved read performance
        - Multi AZ setup for disastor recovery
        - Maintenance windows for upgrades
        - Scaling capabilities (both vertical and horizontal)
        - Storage backed by EBS (gp2 or io1)
    
## RDS Backups
- Backups are automatically enabled in RDS
- Automatic backups:
    - Daily full backup of the database (during the maintenance window)
    - Transaction logs are backup by RDS every 5 minutes
    - Ability to restore to any point in time
    - 7 days retention (can be increased to 35 days)
- DB Snapshots
    - Manually triggered by user
    - Retention of backup for as long as you want

## Read Replicas for read scalability
- Create upto 5 read replicas
- Within AZ, Cross AZ or Cross Region
- Replications are ASYNC so reads are eventually consistent
- Replicas can be promoted to their own DB
- Cost more to have read replicas cross AZ to reduce cost keep the read replica in the same AZ

## RDS Multi AZ (Disaster Recovery)
- SYNC replication
- One DNS name - automatic app failover to standby
- Increase availability
- Failover in case of loss of AZ, loss of network, instance or storage failure
- No manual intervention in apps
- Not used for scaling
- Can setup read replicas to be used as Multi AZ for disaster recovery

## RDS Security - Encryption
- At rest encryption
    - Possibility to encrypt the master & read replicas with AWS KMS - AES-256 encryption
    - Encryption has to be defined at launch time
    - If the master is not encrypted, the read replicas cannot be encrypted
    - Transparent Data Encryption (TDE) is available for Oracle and SQL Server
- In-flight encryption
    - SSL certificate to encrypt data to RDS in flight
    - Provide SSL options with trust certificate when connecting to database
- Encrypting RDS backups
    - Snapshots of unencrypted RDS dbs are un-encrypted, whereas snapshots of encrypted RDS db are encrypted
    - Can copy a snapshot into an encrypted one
- To encrypt an un-encrypted RDS db
    - Create a snapshot of un-encrypted db
    - Copy the snapshot and enable encryption for the snapshot
    - Restore the db from the encrypted snapshot
    - Migrate applications to the new db and delete the old db

## Amazon Aurora
- Aurora is a properietary technology from AWS (not open sourced)
- Postgres and MySQL are both supported as Aurora DB
- Aurora is "AWS cloud optimised" and claims 5x performance improvement over MySQL on RDS, over 3x the performance of Postgres on RDS
- Aurora storage automatically grows in increaments of 10 GB. upto 64TB
- Aurora can have 15 replicas while MySQL has 5, and the replication process is faster (sub 10ms replica lag)
- Failover in Aurora is instantaneous. It's HA native.
- Aurora costs more than RDS (20% more) - but is more efficient
- 6 copies of your data across 3 AZ
    - 4 copies out of 6 for writes
    - 3 copies out of 6 for reads
    - Self healing with peer-to-peer replication
    - Storage is striped across 100s of volumes
- One Aurora instance takes writes (master)
- Automated failover for master in less than 30 seconds
- Master + up to 15 Aurora read replicas serve reads
- Support for Cross Region Replication

## Aurora Serverless
- Automated database instantiation and auto-scaling based on actual usage
- Good for infrequent, intermittent or unpredictable workloads
- No capacity planning needed
- Pay per second, can be more cost-effective

## Global Aurora
- Cross region read replicas
    - Useful for disaster recovery
    - Simple to put in place
- Aurora Global Database (recommended)
    - 1 Primary Region (read/write)
    - Up to 5 seconday (read-only) regions, replication lad is less than 1 second
    - Up to 16 Read replicas per secondary region
    - Promte another region as master within 1 minute