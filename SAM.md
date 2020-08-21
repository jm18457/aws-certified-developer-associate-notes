# Serverless

Projects to do:

1. Serverless CRON job
2. Thumbnail creation

- Servers is new paradigm in which the developers don't have to manage servers anymore.
- at start serverless === FaaS (function as a service)
- Serversless now includes anything managed (databases, messaging, storage etc.)
- Serverless = do not provision, manage servers

## Lambda Overview

- EC2:
  - Continuously running
  - Scaling means intervention to add / remove servers
  - Limited by RAM / CPU
- Lambda:
  - Functions, no servers to manages
  - Limited by time - short executions
  - Run on-demand
  - Scaling automated
- Integrations: api gateway, kinesis, dynamoDb, s3, cloudfront, cloudwatch events eventbridge, cloudwatch logs, sns, sqs, cognito

## Lambda limits - per limits

- Execution:
  - Memory allocation: 128mb - 3008mb
  - Maximum execution: 900 seconds
  - ENV Variables: max 4kb
  - Concurrency executions: 1000 (can be increased)
- Deployment
  - Compressed zip max size 50mb
  - Uncompressed (code + dependancies) 250mb

## Lambda@edge

- You deploy CDN using cloudfront and want global aws lambda alongside. Use lambda@edge.
- You can change 4 requests with lambda@edge:
  - user => cloudfront
  - cloudfront => origin
  - origin => cloudfront
  - cloudfront => user
- Why to use lambda@edge?
  - Website security and privacy
  - dynamic web application at edge
  - SEO
  - intelligently route across origins and data centers
  - bot mitigation at the edge
  - real time image transformation
  - ab testing
  - user auth authorization
  - user prioritization
  - user tracking and analytics

## DynamoDB

- Fully Managed high availablty with replacation across 3 az
- NOSQL
- massive scalling
- fast and consistent in performance
- (Read Capacity Units) RCU:
  - 1 RCU = 1 strongly consistent read of 4kb per second
  - 1 RCU = 2 eventually consistent read of 4kb per second
- (Write Capacity Units) WCU:
  - 1 WCU = 1 write of 1kb per second

## Advanced DynamoDB

- DynamoDB Streams:
  - Changes in DB can end up in stream
  - Lambda can react to stream (react to changes in real time, send email to users etc.)
- Transactions
- On Demand (2.5x more expensive than provisioned capacity)
- Security:
  - Encryption at rest using KMS
  - VPC endpoints available to access DynamoDB without internet
  - Access fully controlled by IAM
- Backup & Restore available
- Global tables
- Amazon DMS to migration to DynamoDB from mongo, oracle, sql, s3 etc.)
- You can launch local DynamoDB on computer for development.

## API Gateway

- AWS Lambda + API Gateway: no infrastructure to manage
- Support for websocket protocol
- Handle API versioning
- handle different environments (dev, test, prod, ...)
- Handle security (authentication, authorization)
- Create API keys, handle request throttling
- Swagger / Open API import to quickly define APIs
- Transform and validate requests and responses
- Cache API responses, generate SDK and API specifications
- Lambda function (integration):
  - Invoke lambda function, easy way to expose rest api backed by AWS lambda
- HTTP (integration):
  - Expose HTTP endpoints in the backend.
  - Example: internal HTTP api on premise, application load balancer
- AWS Service (integration)
  - Expose any AWS api through API gateway.
  - Example: post message to SQS
  - Why: Add authentication, deploy publicly, rate control etc.
- Endpoint types:
  - Edge-Optimized (default): For global clients, use CloudFront edge locations, gateway lives in only one region
  - Regional: For clients in same region
  - Private: Only be accessed from your VPC using an interface VPC endpoint (ENI), use resource policy to define acccess

## API Gateway - Security

- IAM:
  - Great for users / roles already within your AWS account
  - Handle authentication + authorization
  - Leverages Sig v4
- Lambda authorizer
  - Great for 3rd party tokens
  - Very flexible in terms of what IAM policy is returned
  - Handle Authentication + Authorization
  - Pay per lambda invocation
- Cognito User Pools
  - You manage your owner user poool (can be backed by Facebok, Google login etc.)
  - No need to write any custom code
  - Must implement authorization in the backend

## AWS Cognito

- Cognite User Pools:
  - Serverless database of user for your app
  - Simple login: username or email / password
  - Can enable federated identities (Facebook, Google)
  - Sends backa a JSON web token
  - Can be integrated with API gateway for Authentication
- Cognito Federated Identity Pools:
  - Provide direct access to AWS resources from client side
  - Login to FIP, get temporary AWS credentials, these credentials come with pre-defined IAM policy
  - Example: provice temporary access to write to S3 bucket using facebook login
- Cognito Sync (deprecated use AWS AppSync now):

  - Store preferences, configuration, state of app
  - Requires Federated Identity Pool in Cognito

## SAM

- SAM = Serverless Application Model
- Framework for developing and deploying serverless applications
- All the configuration is YAML code
- SAM can help you to run Lambda, API Gateway, DynamoDB locally
- SAM can use CodeDeploy to deploy Lambda Functions
