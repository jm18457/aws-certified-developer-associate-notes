# SQS

- **Producer:** Send message.
- **Consumer:** Receive message.
- **Messages:** Persisted until deleted or timeout.
- **Queue delay:** Amount of time before consumer can see the message after producer sends it.
- **Message Visibility Timeout:** How long message is invisible to other consumers after it has been polled by one consumer.
- **FIFO Queue:** Messages are ordered in same order as producer sent them.
- **Dead letter queues:** Messages that can't be processed are sent here for manual intervention.
- **Fan out pattern:** Combine SNS and SQS to send same message to multiple SQS. Example: s3 bucket object create => sns => multiple sqs.

# SNS

- **PUB / SUB pattern:** As many subscriptions as you want (10_000_000).

# Amazon MQ

- **Overview:** When migrating to the cloud instead of re-enginnering the application to use SQS and SNS use Amazon MQ.

# VPC (Virtual Private Cloud)

- **VPC:** Virtual Private Cloud
- **Subnets:** Tied to an AZ, network partition of the VPC
- **Internet Gateway:** At VPC level, provide internet access.
- **NAT Gateway (aws managed) & NAT Instance:** Provide internet access to private subnets.
- **NACL:** Stateless, subnet rules for inbound & outbound.
- **VPC Flow Logs:** Network traffic logs.
- **VPC Peering:** Connect TWO VPCs with non-overlapping IP ranges, non-transitive
- **VPC Endpoints:** Provide private access to AWS services within VPC
- **Site to Site VPN:** Connect on-premise VPN to AWS though public network.
- **Direct Connect (DX):** Establish a physical connection between on-premise and AWS through private network.

## Three Tier Architecture

- Public (ELB), Private (ASG), Data Subnets (RDS).
- Wordpress example: Public: Multi AZ, Private: AZs and EC2 in them, Data: EFS

# Docker

- **ECS Clusters:** Logical grouping of EC2 instances
- **EKS:** Managed kubernetes by AWS.
- **ECS Agent:** Ran by EC2 instances
- **Task Definitions:** Metadata in JSON form on how to run a Docker Container. ENV variables, network information, image name, port binding, memory and cpu.
- **ECS Service:** Define how many tasks should be run and how they should be run. Can be linked to ELB.
- **ECR:** Private docker image repository. Access is controlled through IAM.
- **Fargate:** AWS Managed EC2 instances. You still create Tasks, Services, Load balancers, Auto scaling etc.
- **ECS Instance Profile:** ECS Service, ECR Service, Cloudwatch permissions
- **ECS Task Role:** Allow task to talk to other AWS services (like s3 bucket).
- **ECS Task placement:** Binpack, Random, Spread,
- **Service Auto Scaling:** Target (cloudwatch metric), Step (based on cloudwatch alarms), Schedule
- **Cluster Capacity Provider:** Cluster auto scaling.

# Elastic Beanstalk

- **Deployment modes:** Single Instance, High Availability
- **All at once (deployment):** stop all, fastest, but has downtime
- **Rolling (deployment):** update a few instances at a time
- **Rolling with additional batches (deployment):** same as rolling but spins new instances
- **Immutable (deployment):** spins up new instances in a news ASG and does full ASG swap
- **Blue / Green (deployment):** two beanstalk environments and use route53 with weighted routing
- **EB CLI:** There is special EB cli (similar to sam and amplify).
- **Lifecycle policy:** Max 1000 application versions. Delete older application versions (based on amount of versions or time).
- **.ebextensions:** Parameters set in UI can be set in `[filename].config` files in `.ebextensions/` and create other AWS resources.
- **CloudFormation:** Elastic Beanstalk uses CloudFormation under the hood. You can define CloudFormation resources in your `.ebextensions/`.
- **Cloning:** Copies everthing except RDS data. Good for testing.
- **Migrations:** Can't clone because clone copies everything. Need to manually create new beanstalk environment and then use CNAME or Route53 for swaps.
- **Single docker:** Does not use ECS. Provide either Dockerfile or Dockerrun.aws.json.
- **Multi docker:** Uses ECS (creates cluster, service, task etc.). Requires Dockerrun.aws.json.
- **HTTPS:** Load certificate to Load balancer (using cli, .env, console)
- **Custom domain name:** Done using Route53 alias.
- **Worker Environment:** Meant for tasks that are long to complete. (Decoupling applications)
