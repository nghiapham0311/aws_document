![[EBS Volume Type - Provisioned IOPS.png]]

There are three types of provisioned IOPS SSD: two which are in general release:
- io1
- io2
- io2 Block Express

**They’re designed for super high performance situations** where lower latency and consistency of that lower latency are both important characteristics

With io1 and io2, you can achieve a maximum of 64,000 IOPS per volume, that’s four time the maximum for GP2 and GP3. With io1 and ios2, you can achieve a 1,000MB/s of throughput. This is the same as GP3 and significantly more than GP2

With io2 Block Express, you can achieve 256,000 IOPS per volume and 4000MB/s of throughput per volume

There is a maximum at the size to performance ratio. For io1, it’s 50 IOPS per GB of size. so this is more than three IOPS per GP for GP2.

For io2, this increases to 500 IOPS per GP of volume size. And for Block Express, this is 1,000 IOPS per GB of volume size

There is a maximum performance which can be achieved between the EBS service and a single EC2 instance.

- The type of volumes: Different volumes have a different maximum per instance performance level.
- The type of the instance
- The size of the instance

You’re going to need multiple volumes to saturate this per instance performance level. With io1 volumes, you can achieve a maximum of 260,000 IOPS per instance and a throughput of 7,500MB/s. It means you’ll need just over four volumes of performance operating at maximum to achieve this per instance limit.

io2 is slightly less, at 160k IOPS for an entire instance and 4,750MB/s. That’s because AWS have split these new generation of volume types. They’ve added Block Express, which can achieve 260k IOPS and 7,500MB per second for an instance maximum

⇒ You need multiple volumes all operating together and think of this as a performance cap for an individual EC2 instance.

These are the maximums for the volume types but you also need to take into consideration for any maximums for the type and size of the instance

One common use case is when you have smaller volumes but need super high performance and that’s only achievable with io1, io2 and io2 Block Express