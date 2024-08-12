*EC2 Instance roles* and *Instance Profiles* are how *applications* running on an EC2 instance can be **given permissions to access AWS resources** on your behalf.

Short Term Temporary credentials are available via the EC2 Instance Metadata and are renewed automatically by the EC2 and STS Services.

![[EC2 Instance Role.png]]

*EC2 instance roles* are roles that *an instance can assume*, and anything running in that instance has the permissions that that role grants.

It starts off with an IAM role, and that role has a permissions policy attached to it. So whoever *assumes* the role gets *temporary credentials* generated and those temporary credentials give the permission that that permissions policy would grant

An *EC2 instance role* allows the *EC2 service to assume that role*, which means that an EC2 instance itself can assume it and gain access to those credentials.

There’s an intermediate piece of architecture, the **instance profile** (deliverd IAM role to EC2 Instance), and this is a **wrapper around an IAM role**. And the *instance profile* is the thing that *allows the permissions to get inside the instance*

When you create an instance role in the *console*, an *instance profile is created with the same name*. But if you use the command line or CloudFormation, you need to *create these two things separately*.

When using the UI and **you think you’re attaching an instance role direct to an instance, you’re not. You’re attaching an instance profile of the same name**. It’s the instance profile that’s attached to an EC2 instance.

We know by now that when IAM roles are assumed, you’re provided with temporary security credentials which expire, but these credentials grant permissions based on the role’s permissions policy

Inside an EC2 instance, these *credentials* are **delivered via the instance metadata**. An application running inside the instance can access these credentials and use them to access AWS resource, such as S3

One of the great things about this architecture is that the credentials available inside the metadata, they’re always valid. EC2 and the secure token service liaise with each other to ensure that the credentials are **always renewed before they expire**. As long as your application inside the EC2 instance keeps checking the metadata it will never be in a position where it has expired credentials.
## Exam Powerup
- Credentials are **inside meta-data**
- iam/security-credentials/**role-name**
- **Automatically rotated** - Always valid
- Should **always be used** rather than adding access keys into instance
- *CLI tools* **will use ROLE credentials automatically**

[[SSM Parameter Store]]
