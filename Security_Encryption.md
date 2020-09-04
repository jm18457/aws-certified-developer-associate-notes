# Security & Encryption

## Encryption 101

- Encryption in Flight (SSL):
  - Data is encrypted before sending and decrypted after receiving
  - SSL certificates help with encryption (HTTPS)
  - Encryption in flight ensures no MITM (man in the middle attack) can happen
- Server Side Encryption at rest
  - Data is encrypted after being received by the server
  - Data is decrypted before being sent
  - It is stored in encrypted form thanks to a key
  - Encryption / Decription keys must be managed somewhere.
- Client Side Encryption
  - Data is encrypted by the client and never decrypted by the server
  - Data will be decrypted by the receiving client.
  - The server should not be able to decrypt data.

## KMS (Key Management Service)

- AWS Managed Keys (seamlessly integrates into AWS services)
- CMK Types:
  -Symmetric: single encryption key, most common
  - Asymmetric: 2 keys public (encrypt) and private (decrypt), use case for outside of AWS
- CloudTrail integration
- Use KMS when sending sensitive information.

## SSM Parameter Store

- Secure storage for configuration and secrets
- Optional Seamless Encryption using KMS
- Serverless, scalable, durable, easy SDK
- Advanced Parameters: TTL to a parameter, can assign multiple policies at a time

## AWS Secrets Manager

- New service of AWS made for storing secrets
- Out of the box forced rotations, synchornize with lambda, rds etc.

## CloudHSM

## AWS Shield

- DDOS protection
- AWS Shield Standard: free for every AWS customer
- AWS Shield Advanced: 3000 per month per organization, against more sophisticated DDOS attacks, 24 / 7 ddos response team, protected against DDOS cost spikes

## AWS WAF (Web Application Firewall)

- Protects your web application from common web exploits (layout 7)
- Deployable on ALB, Gateway, CloudFront
- Define WEB ACL (Access control list), XSS, SQL injections, restrict IP address
- Rate base rules

## Shared Responsibility Model

- AWS responsibility (Security **of** the Cloud):
  - Protecting infrastracture (hardware, software, network facilities)
  - Managed services
- Customer responsibility (Security **in** the Cloud):
  - For EC2 instance, customer is responsible for management of the guest OS (including security patches and updates), firewall, network configuration, IAM etc.
