#### Aurora Key Differences
![[Aurora Architect.png]]
The *Aurora* architecture is very *different* from normal RDS*.* At its very foundation, it uses the base entity of a cluster, which is something that other engines within RDS don’t have.

A cluster is made up of a number of important things.

Firstly, from a compute perspective, it’s *made* up of a *single primary instance*, and then *zero or more replicas*. Now this might seem similar to how RDS works with the primary and the standby replica, but it’s actually very different.

The *replicas within Aurora* can be used for *reads* during normal operations. So it’s *not like the standby replica inside RDS*. The replicas inside Aurora can actually provide the benefits of both RDS multi-AZ and RDS read replicas. So they can be *inside a cluster, and they can be used to improve availability, but also they can be used for read operations* during the normal operation of a cluster.

That alone would be worth the move to Aurora since **you don’t have to choose between read scaling and availability**. Replicas inside Aurora can provide both of those benefits.
#### Aurora Storage Architecture
![[Aurora Architect-1.png]]
Aurora **doesn’t use local storage** for the compute instances. Instead, an **Aurora cluster has a shared cluster volume**. This is storage which is shared and available to all compute instances within a cluster. This provides a few benefits, such as faster provisioning, improved availability, and better performance.

A typical Aurora cluster looks something like this. It functions across a number of AZs. Inside a cluster is a primary instance and optionally a number of replicas. And again these function as fail over options if the primary instance fails. But they can also be used during normal functioning of the cluster for read operations from applications.

The cluster has shared storage which is SSD-based, and it has a maximum size of 128 TIB. And it also has 6 replicas across multiple AZs.

When data is written to the primary DB instance, Aurora synchronously replicates that data across all of these 6 storage nodes spread across the AZs, which are associated with your cluster.

All instances inside your cluster, so the primary and all of the replicas have access to all of these storage nodes.

**From a storage perspective is that this replication happens at the storage level. So no extra resources are consumed on the instances or the replicas during this replication process**

**Another powerful difference between Aurora and the normal RDS db engines is that with Aurora, you can have up to 15 replicas, and any of them can be the fail over target for a fail over operation.**

![[Aurora Architect-2.png]]

> What you’re billed for is a high water mark, the maximum storage that you’ve consumed in a cluster.

**If you go through a process of significantly reducing storage and you need to reduce storage costs, then you need to create a brand new cluster, and migrate data from the old cluster to the new cluster.**
#### Endpoints
![[Aurora Architect-3.png]]
So there are DNS addresses which are used to connect the cluster. Unlike RDS, Aurora clusters have multiple endpoints that are available for an application. **As a minimum, you have the cluster endpoint and the reader endpoint.**

The *cluster endpoint always points at the primary instance*, and that’s the endpoint that can be used for read and write operations.

The *reader endpoint* will also point at the primary instance if that’s all that there is. But *if there are replicas, then the reader endpoint will load balance across all of the available replicas*. This can be used for read operations.

This makes it much easier to manage read scaling using Aurora vs RDS, because as you add additional replicas which can be used for reads, this reader endpoint is automatically updated to load balance across these new replicas.

You can also create custom endpoints. In addition to that, each instance, so the primary and any of the replicas have their own unique endpoint. So Aurora allows for a much more custom and complex architecture vs RDS
#### Cost
![[Aurora Architect-4.png]]
The biggest downsides, is that there isn’t actually a free-tier option. That’s because Aurora doesn’t support the micro instances that are available inside the free-tier.

For any compute that you use, there’s an hourly charge and you’re billed per second with 10 min minimum.

For storage you’re billed based on a gigabyte month consumed metric. Of course taking into account the high water mark. so this is based on the maximum amount of storage that you’ve consumed during the lifetime of that cluster.

There is an IO cost per request made to the cluster shared storage.

In terms of backups, you’re given 100% of the storage consumption for the cluster in free backup allocation. So if your db cluster is 100 GIB, then you’re given 100 GIB of storage for backups as part of what you pay for that cluster.

So for most situations, for anything low usage or medium usage, unless you’ve got high turnover in data, unless you keep the data for long retention periods, in most cases, you’ll find that the backup costs are often included in the charge that you pay for the db cluster itself.
#### Aurora Restore, Clone & Backtrack
![[Aurora Architect-5.png]]

[[Aurora Serverless]]