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
