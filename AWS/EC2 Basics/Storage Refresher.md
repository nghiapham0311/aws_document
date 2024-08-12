#### Key Terms
- **Direct** (local) attached Storage - Storage on the EC2 Host, directly on the host device. In EC2 it is called *Instance Store*. Super *fast*, but when we change host or host is down for maintain, disk fail, storage fail the data is lost.
- **Network** attached Storage - Volumes is created and attached to device over the network (*EBS*). Highly Available, if the hardware failed, the storage is not lost.
- **Ephemeral** Storage - Temporary Storage. => ex: *Instance Storage*.
- **Persistent** Storage - Permanent storage - lives on past the lifetime of the instance. => ex: *EBS*.
#### Storage Categories
![[Storage Refresher.png]]

###### Block Storage
Block storage is *a collection of addressable blocks* presented either logically, *as a volume, or as a blank physical hard drive*.

Block storage comes in *the form of spinning hard disks or SSDs*, so physical media.

The key thing is that block storage has *no in-built structure*. It’s just a *collection of uniquely addressable blocks*. It’s up to the OS to create a file system and then to mount that file system, and that can be used by the OS.

So with block storage in AWS, you can mount a block storage volume, so you can mount an EBS volume, and you can also boot off an EBS volume.

> Most EC2 instances use an EBS volume as their boot volume and that’s what stores the OS, that’s what’s used to boot the instance and start up that OS.
###### File storage
A file storage in the on premises world is provided by *a file server.* It’s provided as a ready made file system with a structure that’s all ready there.

File storage *has structure*. Can be *mount*.

You can take a file system, you can browse to it, you can create folders and you can store files on there. You access the files by knowing the folder structure, so traversing that structure, locating that file and requesting that file.

You *cannot boot from the file storage* because the OS doesn’t have low level access to the storage.
###### Object Storage
Object storage is a very abstract system where you just store objects. There is *no structure*. It’s just a *flat collection of objects, and an object can be anything*. 

It can have attached metadata, but to *retrieve* an object, you generally provide a *key*.

Objects can be *anything*, They can be **binary data, they can be images, they can be movies**.

The benefit of object storage is that is it’s super *scalable*, it can be accessed by thousands or millions of people simultaneously. But it’s generally **not mountable** inside a file system and it’s definitely **not bootable**.

Think of it like S3 which we learn before hand.

**The differences between these three main type of storage**
- If you want to utilize *storage to boot from*, it will be **block storage**.
- If you want to utilize *high performance storage inside an OS*, it will also be **block storage**.
- If you want to *share a file system across multiple different servers or clients* or have them accessed by different services, that can be often be **file storage**.
- If you want *large access to read and write object data at scale*, so if you’re making a web scale application, that is ideal for **object storage**, because it is almost infinitely scaleable.
#### Storage Performance
![[Storage Refresher-1.png]]

There’s the *IO* or **block size**, the **input-output operations per second** (*IOPS*) and the *throughput* - the **amount of data that can be transferred in a given second**

You can think of IOPS as the speed at which the engine of a race car runs at

You can think the IP or block size as the size of the wheels of the race car

You can think of the throughput as the end speed of the race car

⇒ If you want to maximize your throughput, you need to use the right block size and then maximize the IOPS and if either of these three are limited, it can impact the other two.

[[EBS Architect]]
[[Instance Store Volume]]