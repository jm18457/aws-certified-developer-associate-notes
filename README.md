# Study Guide

## IAM

- **IAM:** Identity and Access Management. Global Service.
- **User:**
- **Group:**
- **Policy:** Effect (allow or deny), Action (array of actions to effect), Resource (arn).
- **Role:**
- **STS (Security Token Service):** Grant limited and temporary access to AWS resources. AssumeRole API.
- **Identity Federation:** Allow users outside of AWS to assume temporary role for accessing AWS resources.
- **Organizations:** Group and manage multiple AWS accounts.
- **Organization trails:**
- **RAM:**
- **IAM Permission Boundaries:** Max boundaries that user can get.
- **Allow / Deny:** Explicit deny > explicit allow > implicit deny.
- **IAM conditions:** Restrict by ip, region etc.
- **Access Advisor:** Analyze unused roles.
- **AWS Trusted Advisor:** Real-time guidance for best practices, cost and performance optimizations etc.
- **Access Analyzer:** Identify resources in your organization and accounts that are shared with external entity.
- **Amazon Inspector:** Security and compliance. Vulnerabilies, exposures etc.
- **Trust Policy:** Define which principals can assume role.

# EC2

- **EC2 Instance Connect:** Browser based SSH connectin through AWS console.
- **Security Groups:** They control traffic that is allowed into (inbound) / out (outbound) for EC2 Machines. Can be attached to multiple instances. Outside of EC2 instance. Time Out => security group missconfigured. Connection refused => application error.
- **IPv4:** 256\*\*4 options. Example: 250.250.250.250
- **IPv6:** Example: 1900:4545:3:200:f8ff:fe21:67cf
- **Public IP:** Can be identified from internet. Must be globally unique.
- **Private IP:** Can be identified from private network. Must be unique within private network. Can connect to web using NAT gateway.
- **Elastic IP:** Fixed IP in AWS. Up to 5 per account.
- **EC2 User Data:** Script run with root user at first instance start, used for installing software, updates etc.
- **On Demand Instances:** Hourly, short workload, highest cost.
- **Reserved Instances:** Minimum 1 year, cheaper, long workloads.
- **Scheduled Reserved Instances:** You reserve time window. Example: every friday between 4 - 6.
- **Spot Instances:** Define max spot price. If spot price is lower you get otherwise you lose it (2 min grace period).
- **Dedicated Instances:** Only you are using the hardware.
- **Dedicated Hosts:** Book an entire physical server.
- **Spot Block:**
- **Spot Fleets:**
- **EC2 Instance Types:** R (ram), C (cpu), M (general), I (high I/O), G (gpu), T2 / T3 burstable, T2 / T3 unlimited burst
- **T2 / T3 bursting:** Unexpected spike => burst => if credits are gone cpu becomes bad.
- **AMI (Amazon Machine Image):** Create AMI from EC2 instance. Faster boot time. Preinstalled software etc.
- **Cross Account AMI copy:**
- **Placement Groups:** Cluster (same hardware, low latency group in single AZ), Spread (across AZ), Partition (combination of cluster and spread).
- **ENI:** Logical component in a VPC that represend a virtual network card.
- **Stop:** Data on disk is kept intact.
- **Terminate:** Root EBS volume is destroyed.
- **Hibernate:** In-memory (ram) state is preserved.
- **EC2 CLI and roles:** Never setup CLI on AWS. Create role and attach it to EC2 instance.
- **EC2 Instance Metadata:** Internal IP address with which instance can access information about themself. http://169.254.169.254/latest/meta-data

## Load Balancers

- **TCP:**
- **Scalability** - application can handle greater loads by adapting. Vertical (increase resources), horizontal (increase number of instances).
- **Availability:** Survive disasters.
- **Load balancing:** Balances traffic to underlying instances.
- **ELB:** AWS Managed Load Balancer.
- **Health Checks:** Create endpoint and set interval. If health check fails then load balancer will stop routing to that instance.
- **Classic Load Balancer:** Old, fixed hostname XX.region.elb.aws.com. TCP, HTTPS, HTTP. Need to enable AZ balancing.
- **Application Load Balancer:** Context aware. It examines content of HTTP request before sending it to instance.Has AZ balancing (can't disable).
- **Network Load Balancer:** Context unaware. Layer 4. It just forwards the request without checking it first. Need to enable AZ balancing.
- **Stickiness:** Same client is sent to same EC2 instance behind load balancer. Causes imbalance of calls. For user login sessions.
- **Different capacity instance types:** ELB might route more traffic to instances with higher capacity as to not overload lower capacity instances by equal distribution.
- **SNI:** Loading multiple SSL certificates onto one web server.
- **Cross Zone Load Balancing:** When enabled the load balancer will distribute traffic across AZ, otherwise it is going to distribute only to instances in same AZ as Load balancer.
- **Connection Draining:** Time to complete in-flight requests while the instance is de-registring or unhealthy. Stops sending new requests.
- **Connection:** Load balancers use private address to route traffic to instances. They do not talk to instances through public IP.
- **ALB Request tracing:** Track HTTP requests from clients to targets. When load balancer retrieves request it adds or updates header with trace id.
- **ALB Access logs:**  Detailed information about client request. time, ip address, latencies, paths etc. Optional feature. Stored in s3 bucket.

## Auto Scaling Group

- **Target Tracking Scaling:** Most simple and easy to set-up. Example: I want the average ASG CPU to stay at around 40%
- **Launch configuration:** set ami, min max, load balancers etc.
- **Simple / Step scaling:** Based on CloudWatch alarm.
- **Scheduled Actions:** Time based. Example: Every friday increase amount of instances.
- **Scaling cooldown:** ASG doesn't launch or terminate additional instances before previous scaling takes effect.
- **General:** By default Auto Scaling works only for Ec2 instances. For EB you must change auto scaling group from EC2 to EB.
- **SSL Pass-through:** SSL layer is not decrypted at load balancer but passed along to server.
- **SSL Termination:** Reduces strain on server by offloading decryption.

## EBS - Elastic Block Storage

- **EBS (Elastic Block Storage):** is a network drive, persistant storage (except root).
- **Mount:** Can only be mounted by ONE EC2 instance in same AZ. Not mounted by default, have to mount in ec2 instance. Not mounted after restart, need to fstab.
- **GP2 (SSD):** general purpose price / performance
- **IOI (SSD):** Highest performance for mission critical low latency or high throughput. Databases.
- **STI (HDD):** Low cost for frequently accessed data. Warehouse.
- **SCI (HDD):** Low cost infrequently access data.
- **Snapshots:** Only backup changed blocks. Not while APP running.
- **AZ / Region Migration:** Snapshot the volume, copy volume, create volume from snapshot.
- **Encryption:** Supports in-flight and at-rest encryption.
- **How to encrypt unencrypted:** Snapshot, copy, create.
- **Instance Store (ephemeral):** Physically attached to the machine. Lost on stop. Good for cache.
- **Raid 0:** Split data. Peformance. 2x 500gb with 4000 IOPS => 1x 1000gb with 8000 iops.
- **Raid 1:** Duplicated data. Fault tolerance. 2x 500gb with 4000 IOPS => 1x 500gb with 4000 iops.

## EFS - Elastic File System

- **EFS:** Managed NFS (Network File System).
- **Mount:** Can be mounted by MANY EC2 instances across AZ. Not mounted by default, have to mount in ec2 instance. Not mounted after restart, need to fstab.
- **Use cases:** content management, web serving, data sharing, wordpress
- **Linux only.**

## S3

- **Buckets:** Globally unique name. Store objects (files) in buckets (directories).
- **Objects:** Files. Max size for single upload 100mb. Multi-part upload.
- **Versioning:** Disabled by default. Prevents accidental delete.
- **Encryption:** SSE-S3 (aws managed keys), SSE-KMS (kms managed keys), SSE-C (customer managed keys), CSE (client side encryption)
- **Security:** User, resource based.
- **MFA delete:** Allow delete only with MFA to prevent accidental delete. Needs versioning enabled.
- **Bucket policy:** JSON based policy similar to IAM but for buckets.
- **S3 Websites:** For hosting static websites. Disabled by default. Need to add bucket policy to allow everyont to get objects (unless using cloudfront)
- **S3 Cors:**
- **Consistency:** Eventually consistent. Cannot enforce strong consistency. PUT, PUT, GET if fast enough might return old PUT.
- **Forcing encryption:** Old way: Add bucket policy to allow putObject only with encryption header. New way: Enable default encryption in s3 bucket.
- **Access Logs:** Enable it, then all access logs are sent to specific s3 bucket. Use AWS Athena for analyzing access logs.
- **Replication:** Need versioning. Not chained. Can be different accounts, different regions etc.
- **Pre-Signed URLs:** Can make GET / PUT request by using pre-signed URL.
- **S3 General Purpose:**
- **S3 IA (Infrequent Access):**
- **S3 One Zone IA:**
- **S3 Intelligent Tiering:**
- **CloudWatch logs:** You can export cloudwatch logs to s3 bucket.
- **Glacier 3:** retrieval options: expedited, standard, bulk, minimum storage days: 90
- **Glacier Deep Archive:** 2 retrieval options: standard, bulk, minimum storage days: 180
- **Lifecycle Policies:** Transition actions (s3 => glacier), Expiration actions (delete old versions after time)
- **Notifications:** Actions trigger events (example putObject). Notifications can occur only once if 2 actions happen same time. If you want consistency use versioning.
- **Athena:** AWS Managed service to perform analyics directly against s3 files. Used for access logs.

## CloudFront

- **Overview:** TODO!
- **HTTPS in CloudFormation:** HTTPS is avaialble between client and cloudformation and cloudformation and backend.

## AWS RDS

- **RDS:** AWS Managed serivce. AWS provides: OS patching, continous backups and restore, scaling, multi AZ setup, read replicas, automated provisioning.
- **Backups:** Automatically, default 7 days retention period.
- **Snapshots:** Manually triggered by users. Retention is infinite.
- **Read replicas:** Replication is ASYNC. Eventually consistent. You can make read replica into Multi AZ. Cross region support.
- **Read replicas connection string:** To use read replicas application must update connection string.
- **RDS Multi AZ:** Not for scaling. Sync replication. Used only for increase availability, failover.
- **Encryption:** At rest, In-flight, Network (private subnets, security groups), IAM (manage rds through rds api).
- **IAM auth:** Can be used to login into MySQL and PostgreSQL.
- **Convert to encrypted:** Snapshot => copy snapshot with enable encryptio => restore database from copy of snapshot => migrate applications to new database.

## Amazon Aurora

- **Compatibility:** PostgreSQL and MySQL drivers.
- **Performance:** AWS Cloud optimized, 5x mysql or 3x postgres performance
- **Scalability:** very high.
- **Availability:** very high.
- **Aurora Serverless:** Automated database instantiation and auto-scaling based on actual usage
- **Global Aurora:** Aurora cross region read replicas: usefull for disaster recovery, simple to put in place

## ElastiCache

- **ElastiCache:** AWS Managed Redis or Memcached (in-memory)
- **ElastiCache Security:** SSL. Redis Auth.
- **Redis:** For durability (multi-az).
- **Redis AUTH:** Token. Extra level security.
- **Memcached:** For performance (non persistant).
- **Session Store:** Multi application login. Session data written into ElastiCache.
- **Lazy loading:** All the read data is cached, data can become stale in cache.
- **Write Through:** Adds or update data in the cache when written to a DB.

## Route 53

- **Route53:** is Managed DNS
- **A:** hostname to IPv4
- **AAAA:** hostname to IPv6
- **CNAME:** hostname to hostname, only non root
- **Alias:** hostname to AWS resource, also root
- **DNS Records TTL:** domain => ip is cached.
- **Routing Policy - Simple:** Can't attach health checks. If multiple values then randomly chosen by client.
- **Routing Policy - Weighted:** Example: New deploy receive 1% of requests at the start.
- **Routing Policy - Latency:** Lowest latency for client.
- **Routing Policy - Failover:** If primary healthy then primary else secondary resource.
- **Routing Policy - Geolocation:** Based on user location. If user from UK => select resource closest for UK (we have to specify).
- **Routing Policy - Multi value:** Improved simple. Has health checks.

# AWS Cloudfront

- **AWS CloudFront:** CDN (Content Delivery Network)
- **Origins:** S3 Bucket, custom origin (http) ec2, alb, s3 website etc.
- **Usage:** Caching to improve speed reads.
- **Geo Restriction:** Whitelist, Blacklist.
- **Signed URLs:** URL expiration, users can reach contain with singede url. Has IP range access. Individual files.
- **Signed Cookie:** Same as URL, but for all files.
- **AWS Global Accelerator:** User => AWS Global Acelerator => Edge location => Server through internal network. Otherwise it is passed through external network.

## SQS

- **Max size message:** 1b - 256kb
- **Producer:** Send message.
- **Consumer:** Receive message.
- **Messages:** Persisted until deleted or timeout.
- **Queue delay:** Amount of time before consumer can see the message after producer sends it.
- **Message Visibility Timeout:** How long message is invisible to other consumers after it has been polled by one consumer.
- **FIFO Queue:** Messages are ordered in same order as producer sent them.
- **Dead letter queues:** Messages that can't be processed are sent here for manual intervention.
- **Fan out pattern:** Combine SNS and SQS to send same message to multiple SQS. Example: s3 bucket object create => sns => multiple sqs.

## SNS

- **PUB / SUB pattern:** As many subscriptions as you want (10_000_000).

## SWF (Simple Workflow Service)

- **Overview:** Build applications that coordinate work across distributed components.

## Glue

- **Overview:** TODO

## Kinesis

**MISSING SECTION, CHECK AGAIN!!!!**

## Amazon MQ

- **Overview:** When migrating to the cloud instead of re-enginnering the application to use SQS and SNS use Amazon MQ.

## VPC (Virtual Private Cloud)

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

## Docker

- **ECS Clusters:** Logical grouping of EC2 instances
- **EKS:** Managed kubernetes by AWS.
- **ECS Agent:** Ran by EC2 instances
- **Task Definitions:** Metadata in JSON form on how to run a Docker Container. ENV variables, network information, image name, port binding, memory and cpu.
- **ECS Service:** Define how many tasks should be run and how they should be run. Can be linked to ELB.
- **ECR:** Private docker image repository. Access is controlled through IAM.
- **ECS Container Agent:** TODO, check ecs.config. Proably related to non-fargate.
- **Fargate:** AWS Managed EC2 instances. You still create Tasks, Services, Load balancers, Auto scaling etc.
- **ECS Instance Profile:** ECS Service, ECR Service, Cloudwatch permissions
- **ECS Task Role:** Allow task to talk to other AWS services (like s3 bucket).
- **ECS Task placement:** Binpack, Random, Spread,
- **Service Auto Scaling:** Target (cloudwatch metric), Step (based on cloudwatch alarms), Schedule
- **Cluster Capacity Provider:** Cluster auto scaling.
- **General:** Terminating container instance while in stopped state does not deregister it.

## Elastic Beanstalk

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

## CICD

- **CodeCommit:** Managed & hosted git repository by AWS. Security: IAM, SSH key (created in IAM per user).
- **Code state change events:** CloudWatch events. Occurs when code is modified and push to repo. Example: check for credentials in source code.
- **Repo events:** CloudWatch events. Such as pull request, delete branch etc. trigger SNS.
- **CodePipeline:** List of stages (code, build, test, deploy, provision). State change happen in CloudWatch Events which can in return create SNS notifications.
- **CodeBuild:** AWS Managed build service (alternative to Jenkins). Build instructions can be set in buildspec.yml file. Can be defined in CodePipeline and CodeBuild.
- **buildspec.yml:** Must be root directory of your code. Define environment variables. Phases (install, pre build, build, post build). Outputs artifacts. Can use cache for build speedup.
- **CodeBuild VPC:** By default not in same VPC. You need to specify VPC config. Example: integration testing on database.
- **CodeBuild output encryption:** Can use KMS to encrypt output artifacts.
- **CodeDeploy:** EC2 polling CodeDeploy. CodeDeploy sends appspec.yml. Application is pulled from S3 or github. EC2 run deployment instructions.
- **CodeDeploy deployment groups:** Contains settings and configuration used during the deployment. For Ec2 is contains a list of Ec2 instances.
- **Deployment options (Ec2):** In-place (all existing instances stopped, then new deployed), blue/green
- **CodeDeploy rollback:** Redeploy previously deployed version of the application as new deployment. Rollback first tries to remove all files initiated by deployment on failed instances.
- **Blue / Green ECS:** From old task to updated task in same service. Can specify either canary or linear shift.
- **Blue / Green EC2:** Using ELB, new instances are registered old deregistered.
- **Blue / Green Lambda:** Versions, all are blue green.
- **appspec.yml:** File section, hooks.
- **CodeDeploy Agent:** Needs to be run on EC2 machines.
- **Artifacts:** Are end results of stages. Each stage outputs artifacts and those artifacts are then input into new stage. Using S3.
- **CodeStart:** Integrated solution that groups all above (codecommit, cicd etc.)
- **CodeBuild error:** You can run codebuild locally to debug errors.

## Security & Encryption

- **SSL:** Data is encrypted before sending and encrypted after receving. Protects against MITM attacks.
- **SSE at rest:** Data encrypted after being received by server. Before sent it is decrypted.
- **CSE:** Data is encrypted by the client and decrypted by the client. Server cannot decrypt data.
- **Parameter Store:** Secure storage for configuration and secrets. Integrates with KMS.
- **Secrets Manager:** For storing rotating secrets.
- **KMS:** AWS Managed Keys. Symmetric (single encryption key, most common), Asymmetric (public, private key, use outside of AWS). Max size 4kb. Higher use envelope encryption.
- **AWS Shield:** DDOS protection. Standard tier free for everyone.
- **AWS WAF (Web Application Firewall):** Protect against common web exploits. Deployable on ALP, API Gateway, CloudFront.
- **CloudHSM:**

## CloudWatch Metrics

- **Availability:** For every AWS service.
- **Metric:** Variable to monitor.
- **EC2 instance metrics:** Per 5 minute (or 1 minute for additional cost).
- **Custom Metrics:** PutMetricData API => send custom metrics.

## CloudWatch

- **CloudWatch Dashboards:** Global, graphs from different regions.
- **Sending Logs:** Logs can be sent using SDK.
- **EC2 logging:** Need to run CloudWatch agent on EC2
- **Automatic logging:** Elastic Beanstalk, ECS, AWS Lambda, VPC Flow Logs, API Gateway, CloudTrailer, Route53
- **Log Group:** Usually representing an application.
- **Log stream:** Instance within appllication, container etc.
- **Expiration policies:** never expire, 30 days etc.
- **Alarms:** Trigger notifications for any metric.
- **ClodWatch Events:** Schedules (cron jobs) or event pattern (ex. codepipeline state changes).
- **AWS CloudTrail:** History of all events / API calls made within your AWS account.
- **Config:** Record configuration changes such as security groups, load balance changes etc.

## CloudFormation

- **Infrastructure as code:** Code controls infrastructure.
- **Upload:** To S3 bucket. Cannot edit existing template. Need to upload new version.
- **Deletion:** It deletes all CloudFormation created resources.
- **Resources:** AWS Resources declared in the template.
- **Parameters:** Dynamic inputs for your template.
- **Mappings:** Static / Fixed variables for your template. Usage !FindInMap with provided references.
- **Outputs:** References to what has been created. You can import exported outputs in other CloudFormation templates.
- **Import Outputs:** !ImportValue. Can't delete CloudFormation stack which exports output that is referenced by another template.
- **Conditionals:** List of conditions to perform resource creation. Common: if prod environment then create else not.
- **Metadata:**
- **Helpers:** References and functions.
- **Cross Stack Reference:** CloudFormation template imported exported output from another CloudFormation template.
- **Intrisic Functions:** !Ref (Resources, parameters), !GetAtt (resource attributes), !FindInMap (find in mapping), !ImportValue (import exported outputs), !Join (join values with a delimiter), !Sub (substitute variable in a text), !If, !Not, !Equals
- **Rollbacks (Creation):** If fail all stack creation is deleted.
- **Rollbacks (Update):** If fail all is automatically rollback to previous state.
- **ChangeSet:** Shows all the changes that will happen.

## YAML

- **Key:Value pair**
- **-:** Indicates array item.
- **|:** Indicates multi line string.
- **#:** Indicates comment line.

## Lambda

- **Serverless:** Developers don't have to manage servers.
- **Lambda:** On-demand, short executions, virtual functions. Automated scaling.
- **Synchronous Invocation:** Result is returned right away. (CLI, SDK, API Gateway, ALB)
- **Application Load Balancer:** Target groups: Multi-Header values. (supported by ALB setting)
- **Lambda@Edge:** Deploy Lambda functions alongside your CloudFront CDN. TO DO!
- **Asynchronous invocations:** Caused by S3, SNS, CloudWatch Events... If something goes wrong lambda retries (we need to stop error). Ded Letter queue for failed errors. We can setup this in lambda console.
- **CloudWatch Events / EventBridge:** Executed by either CloudWatch Events (cron jobs) or EventBridge (example CodePipeline changes)
- **S3 Event Notifications:** S3 Event setting under properties/events.
- **Event Source Mapping:** SQS, DynamoDB streams, Kinesis streams. Synchronously. Polling. Batch sizes. TO DO!
- **Destinations:** For asynchornous invocations and event source mapping you can set destinations for success and failure results.
- **Monitoring:** Logs are stored in CloudWatch logs. AWS Lambda requires execution role with permissions to write CloudWatch logs. Metrics for durations, invocations etc. in CloudWatch metrics.
- **VPC:** By default launched in AWS VPC. If in your own VPC you need to setup NAT Gateway for access to internet.
- **Performance:** Timeout: default 3seconds - max 15 minutes. Increasing RAM increases CPU units.
- **Execution context:** Temporary runtime environment. External dependancies, connections (http, db). Maintained for some time in anticipation of another lambda invocation. "keep warm". Includes '/tmp' directory. Next invocation is faster. Define connections outside handler.
- **Temporary space:** /tmp directory max size 512mb.
- **Concurrency:** Up to 1000 concurrent. Higher limit open support ticket.
- **Reserved concurrency:** Limits the maximum concurrency of the function including all aliases and versions.
- **Provisioned concurrency:** You allocate concurrency before increase in invocations. This reduces latency as there is no initializations.
- **Error Concurrency ASYNC Invocation:** Throttled due to max concurrency. Async event goes back and will retry for 6 hours.
- **Cold Start:** New instance => code is loaded and code outside handler ran (init). High latency.
- **Provisioned Concurrency:** Concurrency is located before the function is invoked. Cold start never happens. TO DO
- **External Dependancies:** You need to install and zip dependacies together.
- **CloudFormation:** Inline (no dependancies, very small function) or using S3 zip. Need to provide S3 zip location, with versioning etc.
- **Layers:** Externalize dependancies into layers. TO DO. How to create layers.
- **Versions:** \$LATEST is mutable. Versions are immutable. They get their special ARN.
- **Aliases:** Pointers to Lambda function versions. Mutable. Enable blue / green deployments.
- **CodeDeploy:** It builds on versions and alises. Linear, Canary. Automate deploy from v1 to v2 etc.
- **Limits:** Memory allocation (128mb - 3008mb). Maximum execution: 900 seconds. Environment variables: 4kb. Concurrency: 1000. Deployment sized compressed 50mb (250mb uncompressed).
- **Best Practices:** Perform heavy duty work outside of your handler (database connections, initializations etc.). Use env variables (and encrypt them if secret). Minimize your deployment package by code splitting , layers etc. NEVER have a lambda function call itself recursevily.
- **Cross Account:** Create execution role for lambda and update lambda code to call AssumeRole Api.

## X-Ray

- **Overview:**

## API Gateway

- **Overview:** For exposing lambda functions to the world.
- **Basic features:** API versioning, different environments, security, request throttling, API keys, Swagger / Open API, transform / validate response and requests, caching, generate SDK and API specifications
- **Endpoint Types:** Edge-Optimized (default, for global clients, through cloudfront edge locations), Regional, Private.
- **Stages:** Changes are deployed to stages. They have their own stage variables.
- **Stage variables:** Different variables per stage. Often used to integrade stage with certain lambda alias.
- **Deployment:** TO DO
- **Canary deployments:** % of deployments go to canary (new version)
- **Integration Types:** Mock, HTTP / AWS (lambda, aws services) with mapping templates (example SQS)
- **Lambda proxy:** Function is responsible for request / response. There is no modification of request / response in API gateway.
- **HTTP Proxy:** Same as lambda proxy, but for HTTP.
- **Integration Mappings:** Example. Client => API gateway => soap api => gateway => return json response. To modify response, add headers etc. Can also rename query string parameters etc.
- **Swagger & Open API:** Can import from file or can export existing api as file.
- **Caching:** Per stage, 5min - 1hour. Cache is expensive. Can flush the entire cache. Clients can invalidate cache with header cache control max age 0. Need to disable cache invalidation for any client as that could be abused disaster.
- **Usage plans:** Who can access API stages and methods. How much, how fast they can access them. Uses API keys to identify clients. Configure throttling limits.
- **API Keys:** Order: create APIs, generate API keys, create usage plans, associate API stages and API keys with usage plan.
- **Logging:** Enable at stage level. Logs information about request / response body.
- **Tracing:** Enable X-Ray tracing for extra information.
- **What is X-Ray used for?:** TO DO
- **Metrics:** By stage. CacheHitCount, CacheHitMiss, IntegrationLatency, Latency, 4XX or 5XX errors.
- **CORS:** Must be enabled for different domain. Generic CORS ....
- **Authentication:** Using IAM and IAM Policy. Resource policies. Cognito user pools. Lambda authorizer (Token-Based authorizer). TO DO
- **Lambda Authorizer:** Validates the request by checking sig v4 and checking etc. TO DO

## SAM - Serverless Application Model

## DynamoDB

- **Overview:** AWS Managed NoSQL database. High availability (3 az) and scaling. Auto scaling etc. Made of tables. Each table has primary key.
- **Primary keys:** Partition key (unique), Partition key + sort key (combination unique).
- **Throughput:** Need provisioned WCU and RCU. Can be auto scaled if enabled. Burst credits.
- **WCU:** Write Capacity Units. throughput for writes. One WCU per per second per 1kb of item. Rounds to 1kb increment.
- **RCU:** Read Capacity Units. throughput for reads One RCU per second per 4 kb (2kb if strongly consistent). Rounds to 4kb increment.
- **Consistency:** By default it is eventually consistent, but certain APIs allow strong consistency option.
- **Hot Partition / Hot Key:** RCU / WCU is spread across partitions. We can get exception when only one partition is overloaded.
- **Partions:** TOTAL RCU / 3000 + TOTAL WCU / 1000 equls amount of partions. Hashing algorithm. Event partioning.
- **Basic APIs:** PutItem, UpdateItem, DeleteItem, DeleteTable, BatchWriteItem (part of batch can fail), GetItem, BatchGetItem, Query (partiotion key, sort key, filterexpression), scan (use pagination to keep on reading)
- **Indexes:** LSI (Local Secondary Index) for attributes etc. up to 5. GSI (Global Secondary Index) partition key + optional sort, is new table.
- **Optimistic Concurrency:** Conditional Delete / Update. Ensure that item hasn't changed before deleting it. Two requests with name change. Only one will go through.
- **DAX:** DynamoDB Accelarotor. Seamless cache for db. Writes go through DAX, very low latency increase. AWS Managed service, no managemented needed. Only enable.
- **Streams:** Changes can end up in DynamoDB. Can be read by EC2 or Lambda. You can send keys only, new, used or both. It is not retroactive. For lambda you need to define Event Source Mapping.
- **TTL:** Delete item after an expiry date. No extra charge. Deleted up to 48 after expiration etc. generic ttl index.
- **CLI:** Basic commands: --projection-expression, --filter-expression, --page-size, --max-items, --starting-token
- **Transactions:** Standard SQL transaction, multiple create / update /delete with all or nothing approach. Consumes 2x wcu/rcu.
- **Session State:** Can be used for session state cache (similar to s3, ebs, efs, elasticache). Basically mongodb similar key / value.
- **Partioning Strategies:** If partition key is not unique enough we should add random suffix to it. Instead of 2 partitions we get 20 for example. It scales.
- **Concurrent writes:** Second write overwrited first write if same time.
- **Conditional writes:** Add condition only if value is equal. Then second write will not occur.
- **Atomic writes:** One user increase by 1, the other by 2. In the end by 3.
- **S3 Patterns:** Large Object Pattern. Large data inserted into s3, then metadata returned to db. Client retrieves metadata to get s3. Indexing S3 Object metadata, s3 => lambda => dynamodb.
- **Operations:** Table Cleanup: Drop + Recreate Table. Copying: AWS DataPipeline, Backup & Restore or Scan + Write.
- **Security:** DMS to migrate to different db. Can launch local DyanmoDB. Global Tables. Security: VPC, KMS, TLS, IAM.

## Cognito

- **Overview:**
- **User Pools:** Sign in functionality for users. Integrated with API gateway and ALB. Serverless database of user for app. Containes hosted zone.
- **Lambda triggers:** Authentication, signup, token creation, message. this are synchronous.
- **Identity Pools (federated identity):** Provide AWS credentials to users so they can access AWS resources directly. Integrates with user pools. Uses STS.
- **Sync:** Synhcronize data from device to cognito. Replaced by AppSync.
- **Exercise:** Add Cognito to application (with google login). Add Identity pools application.

## SES

- **Send:** Using AWS SDK or SMTP Interface.
- **Receive:** Integrates with. S3, SNS, Lambda.

## ACM (AWS Certificate Manager)

- **Integrations:** Loads into Load Balancers, CloudFront, APIs on API Gateway.
- **Overview:** AWS Managed certificate.

## Step Functions

- **Overview:** Serverless visual workflow to orchastrate lambda functions. Represent flow as JSON state machines. Sequence, parallel, error handling etc. Order fulfillment, data processing, web applications. Similar to JOBS.

## AppSync

- **Overview:** AWS Managed service that uses graphql. It starts with uploading graphql schema. Applications talk directly to AppSync. AppSync graphql schema resolvers to databases, lambdas, https etc.
- **Security:** API_Key, user pools, openid connect, iam
