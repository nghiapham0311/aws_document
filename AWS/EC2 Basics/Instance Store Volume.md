An _instance store_  provides temporary block-level storage for your instance. This storage is located on disks that are physically attached to the host computer. 

Instance store *is ideal for temporary storage* of information that changes frequently, *such as buffers, caches, scratch data, and other temporary content*, or for data that is replicated across a fleet of instances, such as a load-balanced pool of web servers.

- **Block Storage** Devices
- Physically connected to **one EC2 Host**
- Instances **on that host** can access them
- **Highest storage performance** in AWS
- Included in instance price
- **ATTACHED AT LAUNCH**

Each EC2 host has its own instance store volumes, and they’re isolated to that one particular host. Instances which are on that host can access those volumes

Because they’re locally attached, they offer the highest storage performance available within AWS, much higher than EBS can provide

> You have to attach them at launch time, unlike EBS, you can’t attach them afterwards

![[Instance Store Volume.png]]

If an instance *moves between hosts* then any data that was present on the *instance store volumes is lost* and instances can move between hosts for many reasons.

If they’re stopped and started, this causes a migration between hosts.

Another example is if Host A was undergoing maintenance, then instances will be migrated to a different host. When instances *move between hosts*, they’re given *new blank ephemeral volumes*. Data on the old volumes is lost. They’re wiped before being reassigned, but the data is gone.

Even if you do something like *change instance type*, this will *cause an instance to move between hosts*, and that instance will no longer have access to the same instance store volumes.

One of the primary benefits of instance store volumes is **performance**. You can achieve **much higher levels of throughput and more IOPS** by using instance store volumes versus EBS.

![[Instance Store Volume-1.png]]

## Exam PowerUPs
- Local on EC2 Host
- Add at **launch ONLY**
- Lost on instance **move**, **resize** or **hardware failure**
- High performance
- You pay for it anyway - included in instance price
- **TEMPORARY** data store only
[[Choosing Instance Storage & EBS]]