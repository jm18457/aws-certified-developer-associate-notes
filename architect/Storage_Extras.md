# Storage Extras

## Snowball

- Physical data transport that helps moving TBs or PBs of data in or out of AWS
- Alternative to moving data over network
- Secure
- Pay per data transfer job
- Use cases: large data cloud migrations, disaster recovery
- If it takes more than a week to complete data transfer use snowball
- Direct upload: Client => S3
- Snowball upload: Client => Snowball Client => shipment to AWS Snowball => S3
- Cannot import directly into Glacier, need s3 + lifecycle policy

## Snowball Edge

- Adds computational capability to device
- Example: can load EC2 AMi or Lambda functions on data that is moving

## Storage Gateway

- Hybrid cloud: Half data is on AWS, half data is on premise
- Bridge between on-premise data and cloud data in s3.
- Use cases: disaster recovery, backup & restore, tiered storage
- File gateway: S3 => File gateway => application server => file gateway => application server => s3
- Volume dateway
- Tape gateway

## Native storage AWS

- Block: EBS, Instance Store
- File: EFS
- Object: S3, Glacier

## Amazon FSx for Windows (file server)

- EFS is shared POSIX for linux systems
- Supports SMB protocol and NTFS
- Microsoft Active Directory integration

## Amazon FSx for Lustre

- Linux cluster
- Use cases: Machine Learning, High Performance Computing

## Storage comparison

- S3: Object storage, serverless
- Glacier: Object archival
- EFS: Network file system for linux instances, POSIX filesystem
- FSx for Windows: Network file system for windows servers
- FSx for Luster: High performance computing Linux file system
- EBS Volumes: Network storage for one EC2 instance at a time
- Instance Storage: Physical storage for your EC2 instance
- Storage gateway: File gateway, volume gateway, tape gateway
- Snowball: to move large amount of data to or from cloud phsically
- database: for specific workloads, usually with indexing and querying
