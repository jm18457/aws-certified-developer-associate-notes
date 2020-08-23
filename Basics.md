# Basics

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
