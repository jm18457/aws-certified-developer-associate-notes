# IAM

## IAM Introduction

- IAM = Identity and Access Management
- Concepts: Users, Groups, Roles, Policies
- Root account should never be used.
- Users must be created with proper permissions.
- Policies are written in JSON.
- IAM has predefine "managed policies".
- It's best to give users the minimal amount of permissions they need to perform their job (least privilege principles).
- Is a GLOBAL service.

## IAM Federation (check later what is IAM federation)

- Big enterprises usually integrate their own repository of users with IAM
- This way you can login into AWS using company credentials
- Federation uses the SAML standard

## IAM 101

- One IAM User per PHYSICAL PERSON
- One IAM Role per Application
- IAM Credentials should NEVER BE SHARED
- NEVER write IAM credentials in code.
- Never use the ROOT account except for initial setup.

## Policies

- Statement: consists of below, array of them
  - Effect: Allow or Deny
  - Action: Array of actions to allow
  - Resource: ARN or \*
- IAM Policy Simulator

## STS (Security Token Service)

- Allows to grant limited and temporary access to AWS Resources
- Token is valid for up to one hour
- AssumeRole: for increased security users must assume role instead of being part of groups, cross account access
- AssumeRoleWithSAML: return credentials for users logged in with saml
- AssumeRoleWithWebIdentity: for users logged in with IdP (google, facebook etc.)
- GetSessionToken: MFA, from user or AWS Account root user

## Using STS to Assume a Role

- Create IAM role
- Define which principals can access this IAM role
- Use STS to retrieve credentials and impersonate the IAM role (AssumeRole API)
- Credentials valid (15min - 1hour)

## Identity Federation in AWS

- Federation lets users outside of AWS to assume temporary role for accessing AWS resources.
- These users assume identity provided access role.
- Federations:
  - SAML 2.0 (old way)
  - Custom Identity Broker
  - Web Identity Federation with (without) Cognito
  - SSO (new way)
  - Non-SAML with Microsoft AD
- Using federation you don't need to create users in AWS
- SAML 2.0: Client request to app => app authenticate users => app response to client with saml assertion => client to sts => sts temporary credentials to client => user not has credentials

## Organizations

- Global service
- Allows to manage multiple AWS accounts
- The main account is master account - you can't change it.
- Other accounts are member accounts
- Member Accounts can only be part of one organization
- Consolidated billing across all accounts.
- Multi Account Strategies:
  - Create Accounts per department, strategy etc.

## IAM conditions

- aws:SourceIP: deny if they don't come from predefined lists
- aws:RequestedRegion: restrict the region the API calls are made to
- force MFA
- ...

## IAM Permission Boundaries

- IAM Permission Boundary + IAM Role => attach to user. If role permission not set in Permission Boundary then no permissions for that role
- Explicit deny highest importance. If you have multiple statements if there is deny it is all over. CAnnot be overriden.
- Allow is needed otherwise implicit deny.

## RAM

- Resource Access Manager
