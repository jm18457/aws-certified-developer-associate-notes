## EC2 Storage: EBS && EFS

- EBS is a network drive, persistant storage (except root).
- EBS Volume types:
  - GP2 (SSD): general purpose price / performance
    - Recommended for most workloads.
    - System boot volumes.
    - Virtual Desktops.
    - Low-latency interactive apps.
    - Development and test environments
    - 1gb - 16tb
    - small burst 3000, max burst 16k
  - IOI (SSD): Highest performance for mission critical low latency or high throughput
    - Critical business applications.
    - Large database workloads
    - 4gib - 16tb
    - IOPS = 100 - 64000
  - STI (HDD): Low cost for frequently accessed data.
    - Big data, data warehouses, log processing, throughput optimized
  - SCI (HDD): Low cost infrequently access data.
- Not mounted by default, have to mount in ec2 instance.
- Not mounted after restart, need to fstab.

## EBS Snapshots

- Incremental - only backup changed blocks.
- Shouldn't run while heavy traffic.
- Stored in s3, but wont be visible there.
- Recommended to detach volume when making snapshot.
- Copy snapshots across regions.
- Can make AMI from snapshot.
- EBS volumes restored by snapshots need to be pre-warmed.
- Snapshots can be automated using Amazon Data Lifeclcle Manager.

## EBS Migration

- EBS Volumes are only locked to a specific AZ.
- To migration to different AZ / Region:
  - Snapshot the volume
  - Copy the volume
  - Create volume from snapshot

## EBS Encryption

- Everything is encrypted, data, snapshots, volumes, etc.
- Encryption / descryption you have nothing to do.
- Minimal impact on latency.
- Leverages keys from KMS.
- Snapshots of encrypted volumes are encrypted.
- Encrypt an unencrypted EBS volume:
  - Create snapshot.
  - Copy snapshot and select encrypted.
  - Create volume.

## EBS vs Instance Store

- Instance store is physically attached to the machine. (EBS is a network drive)
- Better I/O performance.
- Good for buffer / cache etc.
- Data survives reboots.
- On stop or termination instance store is lost.
- Can't resize.
- Backups musst be operated by the user.
- Named ephemeral.

## EBS RAID Options

- Needs to be done on OS, not AWS console.
- What is RAID?
- When you need 100 000 IOPS.
- RAID 0 and RAID 1.
- RAID 0 (performance):
  - Instead of having same data on all volumes one volume has 1,2 the other has 3,4 etc.
  - If one disk fails data on that disk is lost.
  - When need lot of IOPS doesn't need fault-tolerance.
  - two 500gb EBS with 4000 iops => one 1000gb RAID with 8000 iops
- RAID 1 (increase fault tolerance):
  - Same data on all EBS.
  - Need fault tolerance
  - two 500gb EBS with 4000 iops => one 500gb RAID with 4000 IOPS

## EFS - Elastic File System

- Managed NFS (Network File System) that can be mounted on many EC2.
- EFS Works with EC2 instance in multi-AZ.
- Highly Available, scalable, expensive, pay per use.
- Use cases: content management, web serving, data sharing, wordpress
- Uses security group to controll access to EFS.
- Compatiable with Linux based AMI
- Encryyption at rest using KMS.
- EFS Scale: grow to petabyte automatically,
- Performance mode: general purpose, latency sensitive
- Performance mode: max i/o
- Storage Tiers: standard, infrequent access (EFS-IA)
- lifecycle management feature

## EBS vs EFS

- EBS:
  - Can be attached to only one instance at a time.
  - are locked at the AZ level
  - gp2: IO increase if the disk size increases
  - io1: can increase IO
  - root get terminated by default
  - migrations: snapshot => copy to different region / az => create volume
- EFS:
  - Mounted to 100s of instances across AZ
  - EFS share website files
  - only for linux instances
  - EFS has higher price point than ebs
  - can leverage EFS-IA for cost savings
- EFS vs EBS vs Instance store
