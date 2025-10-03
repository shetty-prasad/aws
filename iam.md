
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


  what is the difference between permission boundaries and scp in iam ?

“Permissions boundaries limit what a specific IAM user or role can do, while SCPs limit what anyone in an account can do. Permissions boundaries are for delegated administration, SCPs are for organizational governance. Neither grant permissions; they only restrict.”

  Permissions Boundaries vs. SCPs in AWS IAM
1. Scope

**Permissions Boundaries**

Applied at the IAM principal level (user or role).

Acts as a maximum permissions guardrail for that entity only.

It limits what that specific user/role can do, even if they have broader policies attached.

**Service Control Policies (SCPs)**

Applied at the AWS Organization level (accounts, OUs).

SCPs define the maximum allowed permissions across entire accounts or OUs.

They don’t grant permissions — they only restrict.

2. Who They Apply To

**Permissions Boundaries** → Only affect IAM users and roles to which they are attached.

**SCPs** → Apply to all IAM entities in an account (users, groups, roles), including the root user.

3. Purpose

**Permissions Boundaries**

Used for delegated administration.

Example: You let a developer create IAM roles, but enforce that the roles can never exceed certain permissions (e.g., cannot attach AdministratorAccess).

Think of it as a fence for a single IAM identity.

**SCPs**

Used for organizational guardrails.

Example: You want to prevent anyone in the "Dev" OU from creating resources outside us-east-1.

Think of it as a fence around an entire account or OU.

4. Evaluation Logic

For both, AWS evaluates permissions as an intersection of:

Identity-based policy AND

Resource-based policy AND

Permissions boundary (if applied) AND

SCP (if in an Org)

So:
Effective Permission = Identity Policy ∩ Resource Policy ∩ Permission Boundary ∩ SCP

5. Example

**Permissions Boundary Example:**

A developer has a policy with ec2:*.

A permissions boundary says "Action": ["ec2:StartInstances", "ec2:StopInstances"].

Result: Developer can only start/stop EC2, not do all ec2:*.

**SCP Example:**

An account has a policy that denies ec2:TerminateInstances.

Even if a role has AdministratorAccess, it cannot terminate instances anywhere in that account.

6. Key Interview Differentiator

**Permissions Boundaries** = IAM-level max cap for specific users/roles (used for delegation).

**SCPs** = Org-level max cap for all identities in an account (used for governance).

**What’s the difference between IAM Policies, SCPs, and Permissions Boundaries? When to use each?**

Answer (clear contrasts):

Identity-based IAM Policies: Attached to users, groups, or roles. They grant permissions. Use these to define what an individual or role can do.

Resource-based Policies: (e.g., S3 bucket policies) Attached to resources to allow cross-account access or restrict access based on conditions.

Service Control Policies (SCPs): Org-level policies in AWS Organizations that define the maximum permissions accounts can use. SCPs do not grant permissions; they restrict them. Use SCPs for organization-wide guardrails (e.g., forbid ec2:TerminateInstances in prod).

Permissions Boundaries: Applied to an IAM principal to set the maximum permissions that principal can be granted by identity-based policies. They are used to delegate permission creation safely (e.g., allow an admin to create roles but cap what those roles can do).
When to use:

Use IAM policies to grant day-to-day permissions.

Use SCPs for global guardrails across accounts/OUs.

Use Permissions Boundaries to limit delegated admins or automation that creates IAM principals.

Use Resource policies to authorize principals from other accounts or external identities directly on resources.

