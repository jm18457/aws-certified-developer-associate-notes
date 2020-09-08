# AWS RDS

## Overview

- RDS stands for Relational Database Service
- It's a managed DB service for DB use SQL as a query language.
- Create databases managed by AWS in the cloud.
- Following are allowed: postgress, mysql, mariadb, oracle, microsoft sql server
- RDS version deploying DB on EC2:
  - RDS is managed service: automated provisioning, OS patching, continuous backups and restore to specific timestamp, scaling vertical and horizontal, multi AZ setup for Disaster Recovery, read replicas for improved read performance
- RDS backups:
  - automatically enabled in RDS
  - Daily full backup of the database (during the maintenance window)
  - Transaction logs are backed-up by RDS every 5 minutes.
  - Ability to restore to any point in time.
  - 7 days retention (up to 35 days)
- DB Snapshots:
  - manually triggered by user
  - retention is infinite

## RDS Read Replicas for read scalability

- Replication is ASYNC. Reads are eventually consistent.
- Up to 5 read replicates.
- Application must update connection string to leverage read replicas.
- Within AZ, cross Az, cross region.
- Use case: New app wants to start reading your database. This can cause load and reduce performance on production. You create read replica. ONLY used for SELECT.
- Network cost => from one AZ to another, free cost replica same AZ.
- RDS Multi AZ (Disaster Recovery), sync replication, increase availability, failover in case of loss of AZ, does not require update in connection string in apps, not used for scaling. Noone can read / write to it.
- You can make read replica as Multi AZ.

## RDS Security - Encryyption

- At rest encryption:
  - Encrypt master & read repli with AWS KMS
  - Has to be defined at launch time.
  - If master not encrypted read replicas cannot be encrypted.
- In-flight encryption
  - SSL certificates to encrypt data to RDS in flight
  - Provide SSL options with trust certificates when connecting to db
  - To enforce SSL: commands inside database
- Create encrypted:
  - Create snapshot
  - Copy snapshot enable encryption
  - Restore database from encrypted shanpshot
  - Migrate applications to the new database and delete old database
- Network & IAM
  - RDS database usually deployed with a private subnet, not public
  - Security works by leveraging security groups
- Access Management
  - IAM who can manage AWS RDS through the RDS API
  - IAM auth can be used to login into MYSQL and POSTRESQLo

## Amazon Aurora

- Not open sourced
- Compatible with Postgres or Mysql drivers
- AWS Cloud optimized, 5x mysql or 3x postgres performance
- Aurora storage automatically grows in increments of 10gb up to 64tb
- Aurora can have 15 replica sets (5 mysql) and is faster
- Failover in aurora is instantenous
- Autorora costs more than 20%.
- High availability and read scaling
- One Aurora instance takes writes (master)
- Less than 30 seconds for failover.
- Up to 15 replicas.
- Writer endpoint / reader endpoint.
- Security: similar to RDS
- Aurora Serverless:
  - Automated database instantiation and auto-scaling based on actual usage
  - Good for infrequent intermittent or unpredictable workloads
  - Can be more cost-effective, pay per second.
- Global Aurora:
  - Aurora cross region read replicas: usefull for disaster recovery, simple to put in place
  - Aurora Global Database (recommended): 1 primary region, 5 secondary read only regions, decrease latency, recovery time less than 1 minute

## ElastiCache

- ElastiCache is managed Redis or Memcached
- Caches are in-memory database with realy high performance, low latency
- Helps reduce load off of database for read intensive workloads
- Helps make your application stateless
- Write scaling using sharding
- Read Scaling using Read Replicas
- Multi AZ with Failover Capability
- AWS managed service, takes care of OS maintenance / patching, optimizations, setup, configuration, monitoring, failure recovery.
- User Session Store:
  - User logs into application
  - Application writes session data into elasticache
  - User hist another instance of our app and user is already logged in
  - Multi application login
- Redis:
  - multi az with auto-failover
  - data durability using AOF
  - read replicates to scale reads and high availability
  - backup and restore features
- Memcached:
  - non persistant
  - multi-node for partitioning
  - no backup and restore
  - multi-threaded architecture
- ElasticCache - cache security
  - Support SSL in flight encryption
  - Do not support IAM authentication
  - IAM policies on ElastiCache are only used for AWS API-level security
  - Redis AUTH: create token, extra level of security for your cache (on top of security groups)
- Patterns for ElastiCache
  - Lazy loading: all the read data is cached, data can become stale in cache
  - Write Through: Adds or update data in the cache when written to a DB
  - Session Store: store temporary session data in a cache
