# AWS Monitoring Troubleshooting Audit

## CloudWatch Metrics

- CloudWatch provides metrics for every service in AWS
- Metric is a variable to monitor
- Metrics belong to namespaces
- Metrics have timestamps
- Can create CloudWatch dashboards of metrics
- EC2 instance metrics have metrics every 5 minutes
- With detailed monitoring you get data every 1 minute (for cost)
- Detailed monitoring => faster ASG
- Custom metrics:
  - Possibility to define and send your own custom metrics to CloudWatch
  - Ability to use dimensions to segment metrics
  - Use API call PutMetricData

## CloudWatch Dashboards

- Great way to setup dashboards for quick access to key metrics
- Dashboards are global
- Dashboards can include graphs from different regions

## CloudWatch Logs

- Applications can send logs to CloudWatch using the SDK
- CloudWatch can collect log from:
  - Elastic Beanstalk, ECS, AWS Lambda, VPC Flow Logs, API Gateway, CloudTrailer, Route53
- CloudWatch logs can go to:
  - S3 for archive
  - Stream to ElasticSearch
- Log storage architecture:
  - Log groups: arbitrary name, usually representing an application
  - Log stream: instances within application, log files , containers
- Log expiration polices: never expire, 30 days etc.
- Using the AWS CLI we can tail CloudWatch logs

## CloudWatch Logs for EC2 (test this)

- By default no logs from your EC2 machine will to go to CloudWatch
- You need to run a CloudWatch agent on EC2 to push the logs files you want.
- Make sure IAM permissions are correct
- The CloudWatch log agent can be setup on-premises too
- CloudWatch Unitifed Agent: collect logs to send to cloudwatch logs

## CloudWatch alarms

- Alarms are used to trigger notifications for any metrics

## EC2 Instance Recovery

- Statuck check:
  - Instance status: check the EC2 VM
  - System status: check the underlying hardware
- Recovery: ec2 instance => monitor => alarm => ec2 instance recovery

## CloudWatch Events

- Schedula: Cron jobs
- Event Pattern: event rules to react to a service doing something
  - Example: CodePipeline stats changes =>

## AWS CloudTrailer

- Provides governance, compliance and auditor for your AWS Account
- CloudTrailer is enabled by default
- Get a history of events / API calls made within your AWS account by:
  - Console
  - SDK
  - CLi
  - AWS services
- Can put logs from CloudTrail into CloudWatch logs

## AWS CloudWatch vs CloudTrail vs Config

- Cloudwatch: log, analyisis, performance monitoring, event, alerting
- CloudTrail: record api calls made by account
- Config: record configuration changes: security groups, load balance changes, do not expose ports etc., compliance of resource over time
