# RDS Multi AZs and Read Replicas

## Overview
While both will duplicate an existing database. There are two difference types. Multi AZ (Availabilty Zone) and Read Replicas. 

## Multi AZs
Multi AZs will create a duplicate of your RDS and put it in another availability zone in your region. The purpose is to have a fail over just it case of these possible scenarios
- The availability zone in AWS goes offline
- The primary database fails
- The primary database gets rebooted. 

## Read Replicas
Read Replica are a duplicate of the primary database. It is used to reduce the amount of read that happen on your primary database. Since most databases are mostly going to perform reads over write. It's main function is latency The purpose is to improve the performance by
- Having a readable database in another region to speed up users close to the region's geographical location
- reduce the amount of reads to a primary database by offloading to a read replica to improve performance
- A spare database to perform database backups, snapshots
- Can be promoted to be its own database

## Comparison Table
| Multi AZ          | Read Replica         |
|-------------------|----------------------|
| Synchonous        | Asynchronous         |
| Used for Failover | Used for Performance |
| Single Endpoint   | Multi Endpoints      |
