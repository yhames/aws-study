# IAM

- [IAM](#iam)
  - [What's IAM](#whats-iam)
  - [Users and Groups](#users-and-groups)
  - [Permissions](#permissions)
  - [IAM Policies inheritance](#iam-policies-inheritance)
  - [IAM Password Policy](#iam-password-policy)
  - [IAM MFA (Multi-Factor Authentication)](#iam-mfa-multi-factor-authentication)
  - [Access Keys](#access-keys)
  - [IAM Roles](#iam-roles)

## What's IAM

IAM stands for Identity and Access Management, 
which is about `Users and Groups` that who can access AWS resources and what they can do.
It is a global service, not region-scoped.

## Users and Groups

Root account created by default, but it should not be used for shared.

Instead, create `Users` for people within the organization and assign `Groups` to them.
this way, you can manage `permissions` in a more secure and controlled way.

Groups only contain users, not other groups.

Users don't have to belong to a group, and can belong to multiple groups.

![iam_groups.png](images%2Fiam_groups.png)

## Permissions

`Users and Groups` can be assigned JSON Documents called `policies` to define permissions.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "ec2:Describe*",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "s3:*",
      "Resource": "*"
    }
  ]
}
```

In AWS you apply the `least privilege principle` which means you give the minimum permissions required to perform a task.

## IAM Policies inheritance

```json
{
  "Version": "2012-10-17",
  "id": "S3-Account-Permissions",
  "Statement": [
    {
      "Sid": 1,
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::123456789012:root"
      },
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": [
        "arn:aws:s3:::mybucket/*"
      ]
    }
  ]
}
```

* `Version`: policy language version, always 2012-10-17
* `id`: identifier for the policy (optional)
* `Statement`: list of individual statements (required)
  * `Sid`: statement identifier (optional)
  * ★ `Effect`: Allow / Deny
  * ★ `Principal`: account/user/role to which this policy applied
  * ★ `Action`: list of actions this policy allows or denies
  * ★ `Resource`: list of resources to which the actions apply
  * `Condition`: conditions for when this policy is in effect (optional)

## IAM Password Policy

Password Policies in AWS:
  * Minimum password length
  * Require specific character types
    * lowercase, uppercase, numbers, non-alphanumeric
  * Allow all IAM users to change their own passwords
  * Password expiration: require users to change their password after some time
  * Prevent password re-use

## IAM MFA (Multi-Factor Authentication)

> MFA = password + security device

if a password is stolen or hacked, the account is not compromised.

MFA devices in AWS:

* Virtual MFA device (e.g. Google Authenticator, Authy)
* Universal 2nd Factor (U2F) security key (e.g. Yubikey)
* Hardware key fob MFA device (e.g. Gemalto, SurePassID for AWS GovCloud)

## Access Keys

Options to Access AWS:
* AWS Management Console : password + MFA
  * web-based user interface
* AWS Command Line Interface (CLI) : access keys
  * terminal
* AWS Software Development Kits (SDKs) : access keys
  * code

Access keys are used to interact with AWS services programmatically.

Access keys are generated through the AWS Console.
Users manage their own access keys.

Access keys consist of:
* Access key ID (public) -> username
* Secret access key (only shown once) -> password

> AWS CLI is a tool that enables you to interact with AWS services
> using commands in your command-line shell.
> it direct access to the public APIs of AWS services.

> AWS SDKs are available for multiple programming languages
> they enable you to access and manage AWS services programmatically.
> it must be embedded in your application code.  
> e.g. AWS CLI is built on AWS SDK for Python (Boto3)
 
## IAM Roles

Role is specific permissions to perform actions on AWS services.

e.g. EC2 Instance Roles, Lambda Function Roles, etc.

## IAM Security Tools

* IAM Credentials Report (Account-level)
  * report that list all users and the status of their various credentials
* IAM Access Advisor (User-level)
  * shows the service permissions granted to a user and when those services were last accessed
  * this helps you set permissions boundaries and ensure least privilege

## Best Practices

* Don't use root account except for AWS account setup
* One physical user = one AWS user
* Assign permissions using groups
* Create a strong password policy and Use MFA
* Assign permissions of AWS services using IAM Roles
* Use Access Keys for Programmatic Access
* Audit permissions of your account with the IAM Credentials Report
* Never share IAM users and Access Keys

