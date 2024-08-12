*RDS Read Replicas* can be added to an *RDS Instance* - **5 direct per primary instance.**

They can be in the *same region*, or *cross-region* replicas.

They provide **read performance scaling for the instance**, but also **offer low RTO recovery for any instance failure issues**

N.B they don't help with data corruption as the corruption will be replicated to the RR.
![[RDS Read-Replicas.png]]
They provide *performance* benefits for *read operations*. They help us create *cross-region failover capability* and they provide a way for RDS to meet *really low recovery time* objectives, just **as long as data corruption isn’t involved** in a disaster scenario.

Read replicas, are read only replicas of an RDS instance. *Unlike Multi-AZ Instance* where you *can’t by default use the standby replica for anything*, you **can** use read replicas but **only for read operations**

*Multi-AZ Cluster* running in cluster mode, which is the newer version of Multi-AZ is like a combination of the old Multi-AZ instance mode together with read replicas.

**But you have to think of read replicas as separate things. They aren’t part of the main database instance in any way. They have their own db endpoint address and so applications need to be adjusted to use them**

An application, using an RDS instance will have 0 knowledge of any read replicas by default. Without application support, read replicas do nothing. They aren’t functional from a usage perspective. There’s no automatic failover. They just exist off to one side. They’re kept in sync using a synchronous replication.

**Multi-AZ uses synchronous replication and that means that when data is written to the primary instance, at the sam time storing that data on disk on the primary, it’s replicated to the standby.**

And conceptually think of this as a single write operation both on the primary and on the standby.

With Asynchronous data is written to the primary first at which point it’s viewed as committed. Then after that, it’s replicated to the read replicas. This mean in theory, there could be a small lag may be seconds, but it depends on network conditions and how many writes occur on the db.

**Synchronous means Multi-AZ and Asynchronous means read replicas**
#### Performance Improvement
*Read replicas* can be *created in the same region* as the primary db instance, or they *can be created in other AWS regions* known as cross-region read replicas. If you create a cross region read replica then AWS handle all of the networking between regions and this occurs transparently to you and it’s fully encrypted in transit.

![[RDS Read-Replicas-1.png]]
You can create 5 direct read replicas per db instance and each of these provides an additional instance of read performance. So this offers a simple way of scaling out your read performance on a db

Read replicas themselves can also have their own read replicas, but this means that lag starts to become a problem. Because asynchronous replication is used there can be a lag between the main db instance and any read replicas and if you then create read replicas of read replicas, then this lag comes more of a problem.

So while you can use multiple levels of read replicas to scale read performance even more, lag does start to become even more of a problem so you need to take that into consideration.

Additionally, read replicas can help you with global performance improvements for read workloads. So if you have read workloads in other AWS regions, then these workloads can directly connect to read replicas and not impact the performance of the primary instance in any way.
#### RPO/RTO Improvements
![[RDS Read-Replicas-2.png]]
> RPO - Recovery point objectives

> RTO - Recovery time objectives

*Snapshots* and *backups* improve *RPOs*. The more frequent snapshots occur and the better backups are, this offers improved RPO because it limits the amount of data which can be lost, but it doesn’t really help us for RTO because restoring snapshots takes a long time, especially for large db

*Read replicas* offer a near **0 RTO,** and that’s because the data that’s on the read replica is synced from the main db instance. So there’s very little potential for data loss assuming we’re not dealing with data corruption

Read replicas can be promoted quickly. They offer a near 0 RTO. So in a disaster scenario where you have a major problem with your RDS instance, you can promote a read replica and this is a really quick process.

**You should only look at using read replicas during disaster recovery scenarios when you’re recovering from failure. If you’re recovering from data corruption, then logically the read replica will probably have a replica of that corrupted data.**

So read replicas are great for achieving low RTOs, but only for failure and not for data corruption

*Read replicas* are read only until they’re promoted and when they’re promoted you’re able to use them as a normal RDS instance. They’re also a really simple way to achieve **global availability improvements and global resilience** because you can create a cross-region read replica in another AWS region and use this as a failover region if AWS ever have a major regional issue

[[RDS Data Security]]