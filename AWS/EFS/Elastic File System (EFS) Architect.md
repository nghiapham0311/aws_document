Network-based file system which can be mounted within Linux EC2 instances and used by multiple instances at once.
#### EFS
- The EFS service is an AWS implementation of common shared storage standard called NFS (Network File System).
- EFS FileSystem can be **mounted in Linux**.
- **Shared** between many EC instances (mounted on many EC2 instances).
- Private service, via **mount targets** inside a VPC.
- Can be accessed from on-premises - **VPN** or **DX**
#### Architect
![[Elastic File System (EFS) Architect.png]]
Inside *EFS* you create *file systems* and these *use POSIX permissions*.

The EFS file system is made *available* *inside a VPC via mount targets*. And these *run from subnets inside the VPC*. The mount targets have IP addresses taken from the IP address range of the subnet that they’re inside.

To ensure *HA*, you need to make sure that you put *mount targets in multiple AZs*. Just like NAT gateways for a fully HA system, you need to have amount target in every AZ that a VPC users.

You might have an on-premises network, this generally would be connected to a VPC using hybrid networking products, such as VPNs or direct connect and any Linux-based server that’s running on this on-premises environment can use this hybrid networking to connect through to the same mount targets and access EFS file system

![[Elastic File System (EFS) Architect-1.png]]

First *EFS* is for **Linux only instances**. From an official AWS perspective, it’s only official supported using Linux instances.

EFS offers *2 performance modes*, General Purpose and Max I/O.
- *General Purpose* is ideal for *latency sensitive use cases, web servers, content management systems*,. It can be used for home directories or even general file serving as long as you’re using Linux instances. General Purpose is the default.
- *Max I/O*, that can scale to higher levels of aggregate Throughput and operations per second but it does have a trade off of increased latencies. So max I/O mode *suits applications that are highly parallel*. So if you’ve got any application or any generic workload such as *big data, media processing, scientific analysis*, anything that’s highly parallel then it can benefit from using Max I/O

There are also 2 different *Throughput mode*: Bursting and Provisioned.
- *Bursting mode works like gp2 volumes* inside EBS. So it has a burst ball, but the Throughput of this type scales with the size of the file systems. So the more data you store in the file system, the better performance that you get.
- With *Provisioned* you can *specify* *Throughput requirements separately from the amount of data you store*. So that’s more flexible but it’s not the thing that used by default.

Generally you **should pick Bursting**.

EFS file system has *2 storage classes* available:
- *Infrequent Access or IA*, and that storage class is a *lower cost storage* class which is designed for **storing things that are infrequently accessed**. So if you need to store data in a cost effective way but you don’t intend to access it often then you can use IA
- *Standard* is used to store **frequently access files**. It’s also the default and you should consider it the default when picking between the different storage classes.

Conceptually these mirror the trade-offs of the S3 object storage classes. You stem them for data which is used day-to-day and infrequent access for anything which isn’t used on a consistent basis.

Just like S3, you have the ability to use life cycle policies which can be used to move data between classes.
#### FSx for Windows File Server
![[Elastic File System (EFS) Architect-2.png]]
> **In the exam, you need to be looking to identify any Windows-related keywords. So look for things like native Windows file systems, or Active Directory or directory service integration and look for any of the more advanced features.**

**Generally, EFS tends to be used for shared file systems, for Linux EC2 instances, as well as Linux on-premises servers, whereas FSx is dedicated for Windows envs**
![[Elastic File System (EFS) Architect-3.png]]
#### FSx Key Features and Benefits
![[Elastic File System (EFS) Architect-4.png]]
Another thing you need to be aware of is that FSx provides native Windows file system that are accessible over **SMB**

Next is that the product supports **DFS**, which is the distributed file system. So if you see that mentioned either its full name or DFS, then you know that this is going to be related to FSx
#### FSx for Lustre
![[Elastic File System (EFS) Architect-5.png]]
*FSx for Lustre* is a managed *implementation* of the *Lustre file system* which is a file system *designed* specifically *for high performance computing*. It supports **Linux-based instances** running in AWS and also supports POSIX style permissions for file system.

Lustre is designed for use cases such as **machine learning, big data or financial modeling, anything which needs to process large amounts of data and do so with a high level of performance**.

FSx for Lustre can be provisioned using 2 different deployment types. If you have a need for the absolute best performance for short term workloads, then you can pick Scratch.
- *Scratch* is optimized for really high end performance, but it doesn’t provide much in the way of resilience or HA.
- If you need a persistent file system or HA for your workload, then you can choose the *Persistent* option. This is great for longer term storage, it offers *HA*, but crucially, **in one AZ only**.

![[Elastic File System (EFS) Architect-6.png]]


> **If or SMB mentioned, then it’s going to be FSx for windows and not FSx for Lustre.**

> **If you see any mention of Lustre, any mention of really high end performance requirements, any mention of POSIX, high performance computing, machine learning, big data, or any of those types of scenarios, then it’s going to be FSx for Lustre**

> **If you see any mention of machine learning and SageMaker and you need to have access to a really high performance file system, then again it could be FSx for Lustre**

[[AWS Backup]]