#### LC and LT Key Concepts
![[Launch Configuration vs Launch Template.png]]
*LC* and LT *at* a high level, *perform the same task*. They **allow the configuration of EC2 instances to be defined in advance**. Their documents, which let you *configure* things like the *AMI* to use, the *instance type and size*, the *configuration of the storage* which instances use, and they *key pair* which is used to connect to that instance

They also let you define the *networking configuration* and *security groups* that an instance uses. They let you configure the user data, which is provided to the instance and the IAM role, which is attached to the instance, used to provide the instance with permissions.

Everything that you usually define at the point of launching an instance, you can define in launch configurations and launch templates.

**Both of these are not editable**. You define them *once*, and configuration is *locked*.

*LT*, as the newer of the 2, **allow you to have versions**. But for LC, versions aren’t available.

LT also have additional features or allow you to control features of the newer types of instances, things like T2 or T3 Unlimited CPU options, placement groups, Capacity Reservations and things like Elastic Graphics.

**AWS recommend using LT at this point** in time because they’re **a superset of LC**. They provide all of the features that LC provides and more
#### LC and LT Architecture
![[Launch Configuration vs Launch Template-1.png]]
Architecturally, *LT also offer more utility*. LC have one use. They’re used as part of ASG.

ASG offer automatic scaling for EC2 instances and LC provide the configuration of those EC2 instances. They’re not editable nor do they have any versioning capability. If you need to adjust the configuration inside a LC, you need to create a new one and use that new LC .

LT, they can also be used for the same thing, so providing EC2 configuration, which is used within ASG. But in addition, they can also be used to launch EC2 instances directly from the console or the CLI and LT has version.

[[Auto-Scaling Groups]]