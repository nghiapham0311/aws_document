Amazon Machine Images (AMI) 's are the *images* which can *create EC2 instances* of a certain configuration.

In addition to using AMI's to launch instances, you can customize an EC2 instance to your bespoke business requirements and then generate a template AMI which can be used to create any number of *customized* EC2 instances.

Key point:
- AMI’s can be used to **launch EC2** instance
- **AWS** or **Community** Provided
- Marketplace (can include **commercial software** in EC2 instance)
- **Regional** ..**unique ID** e.g. ami-0a8549923f4s3
- Permissions (Public, Your Account is *default*, Specific Accounts to launch EC2 instance from AMI)
- You can create an AMI from an EC2 instance you want to template
#### AMI Lifecycle
![[Amazon AMI.png]]
The first phase is where you use an AMI to launch an EC2 instance. *EBS* volumes are *attached* to EC2 instances using *block device IDs*. The boot volume is usually `/dev/xvda` and an extra volume was called `/dev/xvdf` . These are device IDs and device IDs are how EBS volumes are presented to an instance

So a customized configuration of an EC2 instance, architecturally, will just be the instance and *any volumes that are attached to that instance*, but you can take that configuration and you can use it to create an AMI.

This AMI will contain a few things:
- AMI contains *permissions*, so who can use the AMI, is it public, is it private just to your account, or do you give access to that AMI to other AWS accounts ⇒ that’s stored inside the AMI
- Think of an AMI as a *container*, it’s just a logical container which *has associated information*. It’s an AMI, it’s got an AMI ID and it’s got the permissions restricting who can use it
- When you *create* an AMI for *any EBS volumes which are attached* to that EC2 instance, we *have EBS snapshots created from those volumes*, and remember, EBS snapshots are incremental but the first one that occurs is a full copy of all the data that’s used on that EBS volume. So when you make an AMI, the first thing that happens is the snapshots are taken, and those *snapshots* are actually *referenced* inside the AMI using what’s known as a *block device mapping*
- Block device mapping is essentially just a table of data. It links the snapshot IDs that you’ve just created when making that AMI and it has, for each one of those snapshot, a device ID that the original volumes had on the EC2 instance

When you *launch* an instance *using an AMI* what actually happens is the *snapshots* are used to create new *EBS volumes* in the *availability zone* that you’re launching that instance into, and those volumes are *attached* to that new instance using the *same device IDs* that are contained in that block device mapping.

> So AMIs are a **regional** construct, you can take an AMI from an instance that’s in AZ A and that AMI is an object, is stored in the **region**. The snapshots, are stored on S3 so they’re already regional, and you can use that AMI to deploy instances back into the same AZ as the source instance, or into other AZ in that region.

## Exam Power up
- AMI = **One Region**, only works in that one region
- **AMI Baking** means creating an AMI from a configured instance + application
- An AMI **can’t be edited** .. launch instance, update configuration and make a new AMI
- Can be copied **between regions** (includes its snapshots)
- Remember permission .. **default = your account**

[[EC2 Purchase Options]]

