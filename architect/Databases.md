# Databases

## Overview

- Questions to choose the right database:
  - Read-heavy, write-heavy, balanced?
  - Throughput needs?
  - Will it change?
  - Does it need to scale or fluctuate during the day?
  - How much data to store and for how long?
  - Data model? How will you query the data? Joins, structured, semi-structured?
  - Strong schema? Flexbile schema?

## Database types

- RDBMS: RDS, Aurora - great for joins
- NoSQL database: DynamoDB (~JSON), ElastiCache (key / value pair)
- Object Store: S3 (big objects) / Glacier (archives, backup)
- Data Warehouse: Redship, Athena (SQL analystics)
- Search: ElasticSearch
- Graphsa: Neptune

## RDS Overview

- Managed SQL
- Must provision EC2 instance & EBS Volume type and size
- Support for READ replicas and Multi AZ
- Security though IAM, Security Groups, KMS, SSL in transit
- Backup / Snapshot / Point in time restore feature
- Managed and Scheduled maintenance
- Monitoring through Cloudwatch
- Operations: small downtime when failover happens, when maintenance happens, scaling in read replicas, ...
- Security: AWS responsible for OS secuirty, we are responsible for setting KSM, groups, IAM etc.
- Reliability: Multi AZ, failover in case of failures
- Performance: Doesn't auto scale. Depends on instance type chosen.
- Cost: Pay per hour based on EC2 and EBS

## Aurora

- Auto healing
- Data held 6 replicas across 3 AZ.
- Postgresql / MYSQL compatible API
- Global Read replicas
- Can be global
- Auto Scaling
- Aurora serverless
- Use case: Same as RDS but less maintenance, more performance, more cost.
- Operations: less operations, auto scaling storage
- Security: same as RDS
- Reliability: more than RDS, similar
- Performance: 5x compared to RDS
- Cost: Possible lower than entireprise grade such as oracle

## ElastiCache

- Managed Redis / Memcached (rds for caches)
- in memory data store sub-milisecond latency
- Must provision EC2 type
- Support for clustering, multi az, read replicas
- Securiy same as RDS + Redis AUTH
- Backup same as RDS
- Frequest reads less writes use case

## DynamoDB

- AWS proprietery NoSQL databse
- Serverless, auto scaling etc.
- Can replace elasticache
- DynamoDB streams to integrate with AWS lambda
- Can only query on prim key, indexes or sort key
- Reads can be eventually consistent or strongly consistent
- Use case: serverless apps, small documents (100kb)
- Operations: no operations needed, auto scaling, serverless
- Security: Full securiy through IAm, KMS, SSL
- Reliability: Multi AZ, Backups
- Performance: very good, DAX for caching reads
- Cost: auto scaling

## S3

- Key / Value for objects
- Serverless, high scaling
- Tiers: s3, glacier etc.
- Encryption: SSE-S3, SSE-KMS, SSE-C, client side encryption
- Use case: static files, website hosting etc.
- Operations: no operations needed
- Security: IAM, Bucket Policies, ACL, Encryption, SSL
- Relaiblity: 99,999999999%
- Performance: very high
- Cost: per storage, network cost, request number

## Athena

- Query engine on top of S3 serverless
- Operations: no operations needed, serverless
- Security: IAM + S3
- Reliabiliy: managed service, high availability
- Performance: queries scale based on data size
- Cost: Per query / per TB of data scanned

## Redshift (do later)

- Based on PostgreSQL
- Used for OLTP
- Online Analyticsl Processing

## Neptune

- Graph database
- Use case: high relationship data, social networking, knowledge graphs

## ElasticSearch

- Designed for searching, partial searches etc.
- Open Source but also service on AWS
