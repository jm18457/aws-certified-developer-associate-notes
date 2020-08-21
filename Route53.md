# Route 53

## Overview

- Route53 is Managed DNS
- Most common records:
  - A: hostname to IPv4
  - AAAA: hostname to IPv6
  - CNAME: hostname to hostname
  - Alias: hostname to AWS resource

## DNS Records TTL

- Browser caches response that Route53 returns. domain => IP (this is cached).
- High TTL: less traffic to DNS, possible outdated records
- Low TTL: more traffic on dns, records are outdated for less time, easy to change records
- TTL is mandatory for for each DNS record

## CNAME vs ALIAS

- CNAME:
  - hostname => hostname
  - only non root domain
  - example: ELB domain name => your domain
- Alias: (almost always use alias over cname)
  - Hostname => aws resource
  - works also for root domain
  - Free of charge
  - native health check

## Routing Policy - Simple

- Redirect to a single resource
- Can't attach health checks
- If multiple values random one is chosen by the client

## Routing Policy - Weighted

- Control the \$ of the requests to specific endpoint
- Helpful to test 1% of traffic on new app version
- Helpful to split traffic between two regions
- Can be associated with Health Checks

## Routing Policy - Latency

- Redirect to the server that has least latency close to client call.
- Helpful when latency of users is a priority.
- Latency is evaluated in terms of user to designated AWS region
- Example: Germany might be directed to US (if that is lowest latency)

## Routing Policy - Failover

- Browser => DNS request to Route 53 => Route 53 to primary if health check healthy else secondary

## Routing Policy - Geolocation

- Routing based on user location
- Example: Trafick from UK => Germany etc.
- Should create default policy when no match on location.

## Routing Policy - Multi Value

- Use when routing to multiple resources
- Want to associate a Route 53 health checks with records
- Up to 8 healthy records are returned for each multi value query
- Multi Value is not a substitute for having an ELB

# Instantiating Applications quickly

- When launching a full stack (EC2, EBS, RDS) it can take time to:
  - Install applications
  - Insert initial data
  - configure everyything
  - launch the application
- Speed up:
  - EC2 Instances:
    - Use a Golden AMI: Install your applications, OS dependancies, etc. beforehand and launch your EC2 instance from Golden AMI
    - Bootstrap using User Data: for dynamic configuartion
    - mix Golden AMI and User Data (Elastic Beanstalk)
  - RDS databases:
    - Restore from snapshot: database will have schemas and data ready
  - EBS Volumes
    - Restore from snapshot: the disk will already be formatted and have data
- Typical application has: Route53, ELB, ASG, ElasticCache, RDS etc. setting this by hand is very slow.
- Develop problems on AWs: managing infrastructure, deploying code, configuratin databses, load balancers, scaling concers etc.
- Most web apps have same architecture (ALB + ASG)

## Elastic Beanstalk overview

- ElasticBeanStalk is a developer centric view of deploying an application on AWS
- It uses EC2, ASG, ELB, RDS etc.
- One view that easy to make sense of.
- Full control of config.
- Free, you only pay for underlying services.
- Managed service.
- Three architecture models:
  - single instance deployment: good for dev
  - LB + ASG: great for production or pre-production web applications
  - ASG only: great for non-web apps in production (workers)
- ElasticBeanStalk has three components:
  - Application
  - Application version: each deployment gets assigned a version
  - Environment name (dev, test, prod): free naming
- You deploy application versions to environments and can promote application versions to the next environment
- Rollback feature to previous application version
