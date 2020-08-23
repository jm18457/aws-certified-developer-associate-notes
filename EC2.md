# EC2

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
