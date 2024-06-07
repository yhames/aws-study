# IAM

- [IAM](#iam)
  - [What's IAM](#whats-iam)
  - [Users and Groups](#users-and-groups)
  - [Permissions](#permissions)

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
