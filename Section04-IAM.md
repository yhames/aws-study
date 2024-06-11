# IAM

- [IAM](#iam)
  - [What's IAM](#whats-iam)
  - [Users and Groups](#users-and-groups)
  - [Permissions](#permissions)
  - [IAM Policies inheritance](#iam-policies-inheritance)
  - [IAM Password Policy](#iam-password-policy)
  - [IAM MFA (Multi-Factor Authentication)](#iam-mfa-multi-factor-authentication)

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