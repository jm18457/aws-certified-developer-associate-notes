# Disaster Recovery

## Overview

- Disaster is any event that has a negative impact on company's business continuity or finances.
- Types:
  - On-premise => On-premise, traditional, very expensive
  - On-premise => AWS Cloud: hybrid recovery
  - AWS Cloud Region A => AWS Cloud Region B
- RPO: Recovery Point Objective, how many backups do you do, data loss
- RTO: Recovery Time Objective, between disaster and recovery => downtime

## Strategies

- Backup and Restore: high rpo
- Pilot Light: small version of app is always running, useful for critical core, similar to backup and restore but faster
- Warm Standby: full system is up and running, but at minimum size, upon disaster we can scale to production, route53 failover
- Hot site / multi site approach: Full production scale is running

## Database Migration Service

- Homogenous migration: mysql => mysql
- Not homogenous migration: mysql => aurora
- SCT: Schema Conversion Tool: change db engine, oracle => postgresql

## On-premise strategy with AWS

- Ability to download Amazon Linux 2 AMI
- VM Import / Export: migrate existing applications into ec2
- Application Discovery Service: gather information about your on-premise service for migration preparation
- DMS: migrate database, aws => aws, on-premise => aws, aws => on-premise
- SMS (server migration service): on-premise live servers => aws

## DataSync

- Move large amount of data from on-premise to AWS
- Meant for data files (s3, efs)
- Transfer large amount of data to AWS (200tb):
  - Over the internet: slow 180d
  - Direct connect: faster but still slow 18d
  - Snowbal: snowball takes 1 week
  - On-going replication: DMS, DataSync etc.
