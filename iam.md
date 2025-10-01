
AWS IAM is a web service that helps you securely control access to AWS resources. It allows you to manage who can access (authentication) and what actions they can perform (authorization). IAM is global (not region-specific) and comes at no additional cost.

Core IAM Concepts

Users

* Represent individual identities (e.g., developers, admins).
* Have long-term credentials: passwords and access keys.

Groups

* Collection of IAM users.
* Policies attached to groups apply to all users in the group.

Roles

* Temporary security credentials.
* Used by AWS services, federated users, or applications.

Example: An EC2 instance role grants permissions to applications running on EC2 without hardcoding credentials.

Policies

* JSON documents defining permissions.
* Types:
  * Identity-based policies (attached to users, groups, roles).
  * Resource-based policies (attached to resources like S3 buckets).

Elements of a policy:

* Effect (Allow/Deny)
* Action (API calls like s3:GetObject)
* Resource (ARN of resources)
* Condition (optional fine-grained control)

Federation & Identity Providers

* Enables SSO (Single Sign-On) via SAML, OIDC, or Cognito.
* External users can access AWS without creating IAM users.

Permissions Boundaries

* Advanced feature to set the maximum permissions an IAM entity can have.

Service Control Policies (SCPs) (part of AWS Organizations)

* Restrict what accounts can do at an organizational level.

### A policy is a collection of permissions

![image](https://github.com/user-attachments/assets/dffb4c3e-d13c-4255-9582-ad913b75aebc)

![image](https://github.com/user-attachments/assets/12aaa0cc-996c-45a1-993c-04ccd28f954f)

#### Identity policy can be attached to a role, group or user 

![image](https://github.com/user-attachments/assets/ee77bf4b-3745-40d3-bd28-958c1f388505)

![image](https://github.com/user-attachments/assets/eaed93dc-1aab-4d1a-9b62-febad9dbd3e9)

#### Permission boundary 

![image](https://github.com/user-attachments/assets/9bf337da-3a75-4bd6-89cf-e6d01a7e49e6)

#### Roles 

![image](https://github.com/user-attachments/assets/8545255c-cb92-4263-a057-2794717cc246)

Roles can be attached to resources or users. The permission to those roles come from policies . These policies can be attached to IAM user/groups or resources .

![image](https://github.com/user-attachments/assets/79914b67-4a0a-401f-93e3-6b7a68643b0a)

#### Differences Between IAM Policies and IAM Roles
- IAM Policies
  
Specifies what actions are allowed or denied  

Attached to IAM users, groups, or roles

Credentials are permanent 

- IAM Roles
  
Grants temporary access to AWS resources 

Attached to AWS services or assumed by users

Temporary credentials generated when assumed 

#### Session policies

![image](https://github.com/user-attachments/assets/8f277108-8d96-4c5d-9271-4ef6b1b43a19)

IAM Best Practices

* Follow least privilege principle (grant minimum required access).
* Use IAM roles instead of long-term access keys.
* Rotate access keys regularly if used.
* Enable MFA (Multi-Factor Authentication).
* Use IAM Access Analyzer to identify over-permissive policies.
* Integrate with CloudTrail for auditing.
* Use AWS SSO / Identity Center for centralized workforce identity management.





