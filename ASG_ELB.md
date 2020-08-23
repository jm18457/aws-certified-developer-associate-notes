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
