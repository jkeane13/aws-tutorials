# RDS Multi AZs and Read Replicas

## Overview
While both will duplicate an existing database. There are two different types. Multi-AZ (Availabilty Zone) and Read Replicas. 

## Multi AZs
Multi AZs will create a duplicate of your RDS database and put it in another availability zone in your region. The purpose is to have a fail over just it case of the following scenarios
- The availability zone where your primary databse lives in AWS goes offline
- The primary database fails
- The primary database gets rebooted. 

The endpoint to connect to the database never changes. So when the primary databse goes down. Anything connected to the primary databse will be switched over to the Multi-AZ database

## Read Replicas
Read Replicas are a read duplicate of the primary database. It is used to reduce the amount of read traffice to the primary. Since most databases are mostly going to perform reads over write. It's main function is to reduce latency by freeing reads from the primary databse. The purpose is to improve the performance by
- Having a readable database in another region to speed up users close to the region's geographical location
- Reduce the amount of reads to a primary database by offloading to a read replica to improve performance
- A spare database to perform database backups and snapshots
- To promote a read replica to be its own database

Each read replica has it's own endpoint, where you can control which service will connect to which database.

## Comparison Table
| Multi AZ          | Read Replica         |
|-------------------|----------------------|
| Synchonous        | Asynchronous         |
| Used for Failover | Used for Performance |
| Single Endpoint   | Multi Endpoints      |
