# EC2 & IAM

## AWS Regions

- Region is a cluster of data centers.
- Most services are region-scoped.
- Region names are eu-central-1, us-east-1, etc.
- AWS has regions all around the world.

# AWS Availability zones

- Each region has many availability zones (usually 3, min 2, max 6)
- Each AZ is one or more data centers with power, networking, connectivity.
- They are separate from each other so that they are isolated from disaster.
- Connected with high-bandwidth low latency networking.
- Example: eu-central-1 => eu-central-1a, eu-central-1b, eu-central-1b

## IAM

## IAM Introduction

- IAM = Identity and Access Management
- Concepts: Users, Groups, Roles, Policies
- Root account should never be used.
- Users must be created with proper permissions.
- Policies are written in JSON.
- IAM has predefine "managed policies".
- It's best to give users the minimal amount of permissions they need to perform their job (least privilege principles).
- Is a GLOBAL service.

## IAM Federation (check later what is IAM federation)

- Big enterprises usually integrate their own repository of users with IAM
- This way you can login into AWS using company credentials
- Federation uses the SAML standard

## IAM 101

- One IAM User per PHYSICAL PERSON
- One IAM Role per Application
- IAM Credentials should NEVER BE SHARED
- NEVER write IAM credentials in code.
- Never use the ROOT account except for initial setup.

## EC2

## What is EC2?

- EC2 is one of most popular AWS offering
- It consists of the following capabilities:
  - Renting virtual machines (EC2)
  - Storing data on virtual drives (EBS)
  - Distributing load across machines (ELB)
  - Scaling the services using an auto-scaling group (ASG)
- Knowing EC2 is fundamental to understand how the Cloud works

## EC2 Instance Connect

- Browser based SSH connection
- EC2 instance => connect button => EC2 Instance Connect
- Miss-configured SSH key (not 400) causes permission denied error.

## Security Groups Introduction

- Security Groups are the fundamental of network security in AWS
- They control how traffic is allowed into or out of our EC2 Machines.
- It is the most fundamental skill to learn to troubleshoot networking issues.
- Concepts: allow, inbound and outbound ports.
- Security Groups are acting as a "firewall" on EC2 instances
- They regulate:
  - Access to Ports
  - Authorized IP ranges - IPv4 and IPv6
  - Control of inbound network (from other to the instance)
  - Control of outbound network (from the instance to other)

## Security Groups Good to know

- Can be attached to multiple instances
- Locked down to a region / VPC combination
- Does live "outside" the EC2. If traffic is blocked by security group, then EC2 instance will not see the request.
- If your application is not accessible (time out), then it is probably a security group issues.
- If your application gives a "connection refused" error, then it's application error.
- By default all inbound traffic is blocked except SSH.
- By default all outbound traffic is authorized.

## Private vs Public vs Elastic IP (what is NAT?)

- Networking has two sorts of IPs. IPv4 and IPv6
  - IPv4: 250.250.250.250
  - IPv6: 1900:4545:3:200:f8ff:fe21:67cf
- IPv4 => 256\*4 possible options, running, out, most commonly used today.
- Public IP:
  - Public IP means machine can be identified on the internet.
  - Must be unique across the whole web.
  - Can be geo-located easily.
- Private IP:
  - Private IP means the machine can only be identified on a private network.
  - IP must be unique across private network.
  - Machines connect to WWW using a NAT + internet gateway (a proxy)
  - Only a specified range of IPs can be used as a private IP
- Elastic IP:
  - It is a fixed IP that you own until you delete it.
  - If you start / stop EC2 instance it's public IP address changes. You need to attach elastic IP to avoid this "problem".
  - You can only have 5 Elastic IP in your account.
  - Overall, try to avoid using Elastic IP:
    - They often reflect poor architectural decisions
    - Instead, use a random public IP and register a DNS name to it
    - Use Load Balancer and don't use a public IP

## EC2 User Data

- Script is only run once at the instance first start.
- EC2 user data is used to automate tasks such as:
  - Installing updates
  - Installing software
  - Downloading common files from the internet
  - etc.
- EC2 User Data Script runs with the root user.

## EC2 Instance Launch types

- On Demand Instances: short workload, predictable pricing
  - Pay what what you use (billing per second)
  - Has the highest cost but no upfront payment.
  - No long term commitment
  - Recommended for short-term and un-interrupted workloads, where you can't predict how the application will behave.
- Reserved: (Minimum 1 year)
  - Reserved Instances: long workloads
  - Convertible Reserved Instances: long workloads with flexible instances
    - Can change EC2 instance type.
  - Scheduled Reserved Instances: example - every Thursday between 3 and 6 pm
    - launch within time window you reserve
    - When you require a fraction of day / week / month
  - Up to 75% discount compared to On-demand.
  - Pay upfront for what you use with long term commitment
  - Reservation period can be 1 or 3 years.
  - Reserve a specific instance type.
  - Recommended for steady state usage applications (think database)
- Spot instances: short workloads, for cheap, can lose instance (less reliable)
  - Up to 90% discount compared to On-Demand.
  - Can lose at any time if your max price is less than the current spot price.
  - The MOST cost-effecient instances in AWs
  - **Useful for workloads that are resilient to failure**
    - Batch jobs
    - Data analysis
    - Image processing
  - DO NOT RUN critical jobs or databases.
  - GOOD combo: Reserves instances for baseline + On-Demand & Spot for peaks
- Dedicated Instances: no other customers will share your hardware
- Dedicated Hosts: book an entire physical server, control instance placement
  - Visibility into the underlying sockets / physical cores of the hardware
  - More expensive
  - Useful for software that have complicated licensing model (BYOL)
  - Alloced for your account for a 3 year period reservation

## EC2 Spot Instance

- Up to 90% cheaper compared to On-demand.
- Define max spot price and get the instance while current spot price is smaller than max.
- If current spot price is bigger than max, then you have 2 minute grace period to stop or terminate your instance.
- Other strategy: Spot Block (What does this mean?)
  - "block" spot instance during a specified time frame (1 to 6 hours) without interruptions

## How to terminate Spot Instances?

- You can only cancel Spot Instance requests that are open, active, or disabled.
- Canelling a Spot Request does not terminate instances.
- If you terminate spot instance, then try to cancel request, spot request will automatically launch new instance.
- First cancel spot request, then terminate spot instances.

## Spot Fleets (need to recheck wtf this is)

- Spot fleets = set of Spot Instances + optional On-Demand instances
- The Spot Fleet will try to meet the target capacity with price constraints
- Strategies to allocate spot instances:
  - lowestPrice
  - diversified
  - capacityOptimized
- Spot Fleets allow us to automatically request Spot Instances with the lowest price

## Ec2 Instance Type - Main Ones

- R: applications that needs a lot of RAM - inmemory caches
- C: applications taht need CPU - compute / databases
- M: applications that need balance - web apps, general
- I: applications that need good local I/O instance storage - databases
- G: applications that need GPU - video rendering / machine learning
- T2 / T3 burstable
- T2 / T3 unlimited burst
- TIP: https://www.ec2instances.info

## Burstable Instances (T2/T3)

- overall good performance
- unexpected load (spike) CPU bursts and handles this
- when it bursts it utilities bursts credits
- if all credits are gone the cpu becomes bad
- if machine stops bursting credits are accumulated over time

## T2/T3 unlimited

- You do not lose burst capabilities.
- You pay extra money if you go over credit balance, but you do not lose in performance.
- Be careful of costs as they can go really high.

## EC2 AMIs (Amazon Machine Image)

- The following advantages to using AMI:
  - Pre-installed packages needed.
  - Faster boot time (no need for ec2 user data)
  - Machine comes configured with monitoring / enterprise software
  - Security concerns - control over the machines in the network
- Using public AMIs:
  - Optimized software.
  - Easy to run and configure.
  - Published on amazon marketplace.
  - Some AMIs might be malware infested.
- AMIs are in s3 buckets, private, locked to your account / region.
- You get charged for storing data in s3 per month.
- Can copy it to different region.
- Create image from instance, then from AMI click launch image. Security group is not the same.

## Cross Account AMI Copy

- You can share AMI to different account.
- Sharing an AMI does not affect the ownership of the AMI.
- If you copy AMI that has been shared with your account, you are the owner of the target AMI in your account.
- To copy an AMI that was shared with you from another account, the owner of the source AMI must grant you read permissions for the storage that backs the AMI.
- You can't copy an encrypted AMI that was shared with you from another account, unless keys were shared with you.
- You can't copy an AMI with an associated **billingProduct** code that was shared with you from another account. This includes AMIs from the marketplace. To copy a shared AMI with a billingProduct code, launch an EC2 instance in your account using the shared AMI and then create an AMI from the instance.

## Placement Groups

- Provides control over EC2 instance placement strategy.
- When you create placement group you specify one of the following strategies:
  - Cluster - same hardware, clusters instances into a low latency group in a single AZ
    - great network
    - if rack fails all instances fail at the same time
    - big data job that needs to complete fast, application that needs extremely low latency and high network throughput
  - Spread - spreads instances across underlying hardware - critical applications
    - minimize fail risk, all instances different hardware
    - limited to 7 instances per AZ per placement group
    - critical applications failures must be isolated from each other
  - Partion
    - split by partitions, one partition can have 100s of instances in partition
    - up to 7 partitions per AZ
    - partitions different hardware

## Elastic Network Interfaces (ENI) (What is the purpose of ENI????)

- Logical component in a VPC that represents a virtual network card
- The ENI can have the following attributes:
  - Primary private IPv4
  - one elastic ip per private ip
  - One public IPv4
  - One or more security groups
  - a mac address
- You can create ENI independently and attach them on the fly on EC2 instances for failover
- Bound to a specific AZ

## EC2 Hibernate

- Stop: data on disk (EBS) is kept intact
- Terminate: root EBS volume is destroyed
- start:
  - first: OS boots and EC2 user data is run
  - other: starts, caches get warmed up etc. and that can take time
- EC2 Hibernate:
  - in-memory (ram) state is preserved
  - instance boot is much faster
  - entire ram state is written to a file in root ebc volume
  - root EBS volume must be encrypted
- long-running processing
- does not support all instance families
- ram size must be less than 150gb
- not suported for bare metal instances
- must be EBS, encrypted, large
- available for on-remand and reserved instances
- cannot hibernate more than 60 days

## Very important

- EC2 instances are billed by the second, t2.micro is free tier
- SSH is on port 22, lock down the security group to your IP
- Timeout issues => Securiy group issue
- Permission issue on SSH => chmod 400 not correct ssh permissions
- Security groups can reference other security groups
- Know the difference between private, public, elastic IP
- You can customize EC2 instance at boot time using EC2 user data
- 4 EC2 launch modes: on-demand, reserved, spot, dedicated hosts
- Know the basic instance types: R, C, M, I, G, T2/T3
- Can create AMIs to pre-install => faster boot
- AMI can be copied between regions and accounts
- Ec2 instances can be started in placement groups
  - cluster
  - spread
  - partition (mix of cluster / spread)

## Scalability and Availability

## Basics

- Scalability - application can handle greater loads by adapting.
- Two kinds of scalability:
  - Vertical - increase resources for one instance
  - Horizontal - more instances, distributed systems
- High availability: goal is too survive data center, auto scaling

## Load balancing

- Load balancers are servers that forward traffic to multiple servers downstream.
- Why use load balancer?
  - Spread load across multiple instances.
  - Expose single point of access to your application.
  - Seamlessly handle failures of downstream instances.
  - Do regular health checks to your instances.
  - Provide SSL termination for your websites.
  - High abailability across zones.
  - Separate public traffic from private traffic.
  - Engorce stickiness with cookies.
- Why use ELB?
  - It is managed load balancer by AWS.
  - AWS guarantees that it will be working, takes care for maintaince upgrades, provides a few configs.
  - Costs more than manual setup, but works better.
  - Integrated into AWS.
- Health checks.
  - Crucial for load balancers.
  - Create endpoint, example /ping.
  - If not 200 response then unhealthy, else healthy.
  - Every 5 seconds.
- Types:
  - Classic Load Balancer (v1)
  - Application Load Balancer (v2)
  - Network Load Balancer (v2)
- User => Load balancer sec group 443 / 80 => EC2 sec group source load balance sec group
- LBs can scale but not instanteously
- 4xx errors are client induced errors
- 5xx application induced errors
- Load Balancer Errors 503 means at capacity or not registered target.
- ELB access logs will log all access requests. (how to view?)
- CloudWatch Metrics will give you aggregate statistics.

## Clasic Load Balancers

- Supports TCP, HTTP, HTTPS.
- Fixed hostname XX.region.elb.aws.com

## Application Load Balancers

- Layer 7 (HTTP)
- Load balancing to multiple HTTP applications across machines (target groups)
- Load balancing to multiple applications on the same machine (containers)
- Support for HTTP/2 and WebSocket.
- Support redirects (from HTTP to HTTPS for example)
- Routing tables to different target groups:
  - Routing based on path in URL
  - Routing based on hostname in URL
  - Routing based on query string
  - Routing based on Headers
- Great fit for micro services and container based application
- Port mapping feature to redirect to a dynamic port in ECS.
- Example we have single AELB. two different routes go through same balancer, then balancer forwards to 2 different target groups based on routes.
- Fixed hostname.
- Application servers dont see the IP of the client directly. X-Forwarded-For contains the IP

## Network Load Balancers

- Layer 4
- TCP & UDP
- Handle millions of request per seconds
- Less latency 100ms vs 400ms for ALB
- One static IP per AZ and supports assigning elastic IP (CLB and ALB don't have static IP, they have static hostname)

## Load Balancer Stickiness

- Same client is always redirected to the same instance behind a load balancer.
- This works for CLB / ALB.
- Use case: make sure user doesn't lose his session data.
- Stickiness may bring imbalance to the load the backend EC2 instances.
- Done via cookie with expiration date.

## Cross-Zone Load Balancing

- Without AZ load balancing happens only for specific AZ.
- CLB: No charge, disabled by default.
- ALB: Always on, can't be disabled.
- NLB: Disabled by default, pay extra for inter AZ.

## SSL - Server Name Indication

- SNI solves the problem of loading multiple SSL certificates onto one web server.
- Newer protocol and requires the client to indicate the hostname of the target server in the initial SSL handshake.
- Only works for ALB, NLB, Cloudfront

## ELB - Connection Draining

- CLB: Connection Draining
- Target GRoup: Deregistration Delay (ALB & NLB)
- Time to complete in-flight requests while the instance is de-registring or unhealthy.
- Stops sending new requests to the instance which is de-registering.
- New connections are forwarded to healthy instances.
- Deregistration delay: 1 - 3600 seconds, default 100. During this time in draining mode, waiting for existing connections to complete. 0 disabled

## Auto Scaling Group

- Can scale out.
- Can scale in.
- Ensure we have max / min running machines.
- Automatically Register new instances to a load balancer.
- Have the following attributes:
  - Launch configuration:
    - ami + instance type, ec2 user data, ebs volumes, security groups, ssh key pair
    - min / max size and initial capacity
    - network + subnets information
    - load balancer information
    - scaling policies
- Auto scaling alarms:
  - cloutwatch alarms
  - check metrics such as average cpu
  - based on alarm create scale out / in policies.
- Auto Scaling new rules - directly managed by ec2
  - target average cpu usage, average network in, average netowrk out etc.
- Auto Scaling Custom Metric
  - Example: number of connected users
  - Send custom metric from EC2 to cloudwatch (putmetric api)
  - create cloudwatch alarm to react to low / high values
  - use cloudwatch alarm as the scaling policy for ASG
- Difference between launch configuration / template.

## Scaling Policies

- Target Tracking Scaling:
  - Most simple and easy to set-up
  - Example: I want the average ASG CPU to stay at around 40%
- Simple / Step scaling
  - Example: When a cloudwatch alarm is trigged then add 2 units
- Scheduled Actions
  - Anticipate a scaling based on known usage patterns
  - Example: increase min capacity to 10 at 5pm on fridays

## Scaling Cooldowns

- ASG doesn't launch or terminate additional instances before previous scaling takes effect.

## ASG For architects

- Find the AZ with most number of instances.
- If there are multiple instances in AZ choose with oldest launch configuration.
- Lifecycle hooks: performance extra steps pending => hooks => inService. terminating => hooks (example want to extract logs before termination) => terminated
- Launch template newer than launch config. Recommended by AWS, multiple features over configuration.

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
