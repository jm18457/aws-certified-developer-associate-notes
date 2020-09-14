# AWS Cloudfront

## Overview

- CDN (Content Delivery Network)
- Improves read performance, content is cached at the edge
- 216 Point of Presence globally (edge locations)
- DDoS protection, integrations with Shield, AWS WEb Application Firewall
- Can expose external HTTPS and can talk to internal HTTPS backends.

## Origins

- S3 Bucket:
  - For distributing and caching them at the edge.
  - Enhanced security with CloudFront Origin Access Identity

- Custom Origin (HTTP):
  - Application Load Balancer
  - Ec2 instance
  - S3 website
  - any http backend you want

## How it works

- Client makes HTTP request with url, headers => reaches cloudfront edge location => reaches origin => edge location receives reponse and caches based on our criteria => returns to client

## Geo Restriction

- Can restrict who can access your distribution
- Whitelist, Blacklist
- Example: Copyright laws

## CloudFront Signed URL / Signed Cookies

- Distribute paid shared content to premium users over the world.
- Attach policy with:
  - Includes URL expiration
  - Includes IP ranges to access the data from
  - Trusted signers (which AWS accounts can create signed URLs)
- How long should URL be valid for:
  - Share content (movie, music): make it short
  - Private content (only for user): years
- Signed URL = access to individual files
- Signed Cookie = get one for all files

## AWS Global Accelerator

- You have deployed an application that wants to be accessed around the globe but your server is only in one location.
- It will talk directly to Edge location and from edge location internal AWS network to our server.