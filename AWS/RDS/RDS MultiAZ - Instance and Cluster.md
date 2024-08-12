*MultiAZ* is a feature of RDS which provisions a **highly available** instance set.

The product provides *MultiAZ instance* .. where a *standby replica* is kept in **sync Synchronously** with the *primary instance*.The standby replica **cannot be used for any performance scaling** ... only **availability**.

it also provides *MultiAZ cluster* mode, where **a write and two reader instances are kept in sync Synchronously**. The *reader instances* can be used **for read operations ..allowing for limited read scaling.**

Backups, software updates and restarts can take advantage of MultiAZ to reduce user disruption.
#### Multi AZ - Instance
![[RDS MultiAZ - Instance and Cluster.png]]
With this architecture, RDS has a *primary database* instance containing any databases that you create, and when you *enable Multi-AZ mode*, this primary instance is configured to *replicate its data synchronously to a standby replica* which is **running in another AZ**. This means that this standby also has a copy of your databases.

In Multi-AZ instance mode, this *replication is at the storage level*. This is actually less efficient than the cluster Multi-AZ architecture.

How this works is that all accesses to the dbs are via the db CNAME. This is a DNS name, which by default points at the primary db instance.

With Multi-AZ instance architecture, you always access the primary db instance. There’s **no access to the standby** even for things like reads. Its job is to simply sit there until you have a failure scenario with the primary instance. Other things though, such as **backups can occur from the standby, so data is moved into S3 and then replicated across multiple AZs in that region**.

**This place is no extra load on the primary because it’s occurring from the standby. This is important because all accesses so reads and writes from this architecture will occur to and from the primary instance**

![[RDS MultiAZ - Instance and Cluster-1.png]]

In the event that anything happens to the primary instance, this will be *detected by RDS and a failover will occur*. This can be done manually if you’re testing or if you need to perform maintenance, but generally, this will be an automatic process.

What happens in this scenario is the *db C name changes*. *Instead of pointing at the primary*, it *points at the standby which becomes the new primary*. Because this is a DNS change, it generally *takes between 60 to 120* seconds for this to occur so there can be brief outages.

This can be **reduced** by **removing any DNS caching in your application** for this specific DNS name. If you do remove this caching, it means that the second RDS has finished the failover and the DNS name has been updated. Your application will use this name, which is now pointing at the new primary instance.

replication between primary and standby is *synchronous* and what this means is that **data is written to the primary and then immediately replicated** to the standby before being viewed as committed.

![[RDS MultiAZ - Instance and Cluster-2.png]]
**Multi-AZ with the instance architecture means that you only have 1 standby replica**

It’s only 1 standby replica, and this standby replica cannot be used for reads or writes. Its job is to simply sit there and wait for failover events.
#### Multi AZ - Cluster
![[RDS MultiAZ - Instance and Cluster-3.png]]
The differences between *Multi-AZ cluster* for RDS and *Aurora*.

With this mode, RDS is capable of having **one writer replicate to 2 read instances**. This is a key difference between this and Aurora.

With this mode of RDS of Multi-AZ, you can have 2 readers only. These are in different AZs than the writer instance, but there will be only be 2. **Whereas with Aurora, you can have more**.

A difference between *this mode of Multi-AZ and the instance mode* is that these **readers are usable**. You can think of the writer like the primary instance within Multi-AZ instance mode in that it can be used for writes and read operations. The reader instances, can be utilized while they’re in this state. They can be *used only for read operations*. This will need application support since your application needs to understand that it can’t use the same instance for reads and writes, but it means that you can use this Multi-AZ mode to scale your read workloads.

In terms of replications between the writer and the readers, well data is sent to the writer and it’s viewed as being committed when at least one of the readers confirms that it’s been written. It’s resilient at that point across multiple AZs within that region.

The cluster that RDS creates to support this architecture is different in some way and similar in others versus Aurora.

In this mode, **each instance still has its own local storage** is different than Aurora. Like Aurora though, you access the cluster using a few endpoint types. First is the *cluster endpoint,* and you can think of this like the database CNAME in the previous Multi-AZ architecture. It *points at the writer* and *can be used for reads and writes* against the database or administration functions.

Then, there’s a *reader endpoint* and this *points at any available reader* within the cluster. In some cases, this does include the writer instance. Remember, the writer can also be used for reads.

In general operation though, this reader endpoint will be *pointing at the dedicated reader instances* and this is how reads within the cluster scale, so *applications can use the read endpoint to balance their read operations across readers within the cluster*.

There are *instance endpoints* and *each instance* in the cluster gets *one* of these. Generally it’s not recommended to use them directly, as it means any operation won’t be able to tolerate the failure of an instance because they don’t switch over to anything if there’s an instance failure ⇒ **Only use these for testing and fault finding**.

![[RDS MultiAZ - Instance and Cluster-4.png]]
RDS using Multi-AZ in cluster mode means 1 writer and 2 reader DB instances in different AZs

In addition, Multi-AZ cluster mode runs on much faster hardware. So any writes are written first to local super fast storage and then flushed through to EBS. So this gives you the benefit of the local super fast storage in addition to the availability and resilience benefits of EBS.

In addition, when Multi-AZ used cluster mode, then readers can be used to scale read operations against the db. So if your applications support it, it means that you can set read operations to use the reader endpoint which frees up capacity on the writer instance and allows your RDS implementation to scale to high levels of performance versus any other mode of RDS

[[RDS Automatic Backup, RDS Snapshots and Restore]]