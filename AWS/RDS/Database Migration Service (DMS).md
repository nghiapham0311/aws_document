![[Database Migration Service (DMS).png]]
It’s *essentially* a managed db migration service. The concept is simple enough. It starts with a replication instance which runs on EC2, this instance runs one or more replication tasks. You need to define a source and destination endpoints, which point at the source and target db.

The **only real restriction** with the service is that **one at the end points must be running within AWS**.

You **can’t** use the product for migrations between two on-premises db.

![[Database Migration Service (DMS)-1.png]]
> If you see any form of db migration scenario, as long as one of the db is within AWS, and as long as there are weird db involved, which aren’t supported by the product, then you can **default to using DMS**.

> If the question talks about **no downtime migration**, then you absolutely should **default to DMS**
#### Schema Conversion Tool (SCT)
![[Database Migration Service (DMS)-2.png]]
#### (DMS) & Snowball
![[Database Migration Service (DMS)-3.png]]
So DMS can often be involved with large scale db migration, so things which are multi terabytes in size. For those types of projects, it’s often not optimal to transfer the data over the network.

Migration to a device and ship back to AWS

**SCT only used for migrations when the engine is changing. The reason why SCT is used here is because you’re actually migrating a db into a generic file format, which can be moved using snowballs. So this doesn’t break the rule of only doing it when the db engine changes because you are essentially changing the db, you’re changing ti from whatever engine the source users, and you’re storing in it a generic file format for transfer through to AWS on a snowball device**

[[AWS Secrets Manager]]