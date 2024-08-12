![[Relational Database Service (RDS) Architecture.png]]
With RDS, you *pay* for and **receive a db server**, so it would be more accurate to call it a database server as a service product.

**Multiple databases** on *one DB Server* (Instance).

This matters because it means that on this db server, or instance, which RDS provides a managed version of a database server that you might have on-premises, only with RDS, you don’t have to manage the hardware, the OS or the installation as well as much of the maintenance of the DB engine, and RDS runs within AWS

You might see *Amazon Aurora* discussed commonly along with RDS, but this is *actually a different product*. Amazon Aurora is a *custom db engine* and product created by AWS which has compatibility with some of the above engines, but it was designed entirely by AWS. In summary, RDS is a managed db server as a service product. It provides you with a db instance, so a db server which is largely managed by AWS.

You *don’t have access to the OS or SSH access*. But there is a variant of RDS called *RDS Custom* where you do have some more *low-level access*.
#### Architecture
![[Relational Database Service (RDS) Architecture-1.png]]
*RDS* is a service which *runs within a VPC*, it needs to operate in subnets within a VPC in a specific AWS region.

*RDS Subnet group* is something that you *create* and you can think of this **as a list of subnets which RDS can use for a given db instance or instances**.

Let’s say I add 2 public subnets, and in the bottom db subnet group, let’s say three private subnets. So in launching an RDS instance, whether you pick to have it highly available or not, you need to pick a DB subnet group to use.

Let’s say that I picked the bottom db subnet group and launched an RDS instance and I chose to pick one with HA so it would pick one subnet for primary instance and another for the standby. It picks at random unless you indicate a specific preference, but it will put the primary and standby within different AZs.

Because the db instances are within private subnets, it means that they would be accessible from inside the VPC or from any connected networks such as on-premises networks connected using VPNs or Direct Connect, or any other VPCs which appeared with this one.

I could also launch another set of RDS instances using the top db subnet group and the same process would be followed. Because there are public subnets, we could also elect to make these instances accessible from the public internet by giving them public addressing and this is something which is really frowned upon from a security perspective, but it’s something that you need to know is an option when deploying RDS instances into public subnets.

You can use a single DB subnet group for multiple instances, but then you’re limited to using the same defined subnets.

If you wanted to split databases between different sets of subnets, then you need multiple DB subnet groups.

**RDS instances can have multiple databases on them.**

**every RDS instance has its own dedicated storage provided by EBS. So if you have a Multi-AZ pair, primary and standby, each has their own dedicated storage. This is different than how Amazon Aurora handles storage**

For RDS, each instance has its own dedicated DBS-provided storage. If you choose to use Multi-AZ as in this architecture, then the primary instances replicate to the stand by using synchronous replication. This means that the data is replicated to the standby as soon as it’s received by the primary.

You can also decide to have Read Replicate. In summary, Read Replicates use asynchronous replication and they can be in the same region, but also other AWS regions. These can be used to scale read load or to add layers of resilience if you ever need to recover in a different AWS region.

**We also have backups of RDS. It’s to an AWS managed S3 bucket, so you don’t see the bucket within your account, but it does mean that data is replicated across multiple AZ in that region.**

So if you have an AZ failure, backups will ensure that your data is safe. If you use Multi-AZ mode, then backups occur from the standby instance, which means no negative performance impact.
#### Costs
![[Relational Database Service (RDS) Architecture-2.png]]
Because it’s a db server as a service product, you’re not really billed based on your usage, instead, like EC2, which RDS is loosely based on, you’re **billed for resource allocation** and there are a few different components to RDS cost architecture.

First, you’ve got the *instance size and type*. Logically, the bigger and more feature-rich the instance, the greater the cost and this follows a similar model to how EC2 is billed. The fee that you see is an hourly rate, but it’s billed per second

Next, we have the *choice of whether Multi-AZ is used or no*t. Because Multi-AZ means more than one instance, there’s going to be additional cost. How much more cost depends on the Multi-AZ architecture

Next is a *per gig monthly fee for storage*, which means the more storage you use, the higher the cost. And certain types of storage such as provisioned IOPS cost more, this is aligned to how EBS works because the storage is based on EBS

Next is the *data transfer cost*, this is a cost per gig of data transfer in and out of your DB instance, from or to the internet and other AWS regions

Next we have *backups and snapshots*. So you get the amount of storage that you pay for the database instance in Snapshot storage for free. So if you have 2 TB of storage, then that means 2TB of snapshots for free. Beyond that, there is a cost and this cost is gig per month of storage, so the more data is stored, the more it costs, the longer it’s stored, the more it costs. 1TB for 1 month is the same cost as 500GB for 2 months, so it’s a per GB month cost.

Finally, we have any *extra costs based on using commercial DB engine types*

[[RDS MultiAZ - Instance and Cluster]]