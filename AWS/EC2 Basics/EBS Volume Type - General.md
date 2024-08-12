General Purpose Volume Type using SSD
#### GP2
![[EBS Volume Type - General.png]]

When you first create a GP2 volume it can be *as small as one GB or as large as 16TB*. And when you create it, the volume is created with an *IO credit allocation*.

Think of this like a bucket. So an **IO is one input output operation**.

An IO credit is a 16KB chunk of data. So an IOP is one chunk of 16Kb in one second.

If you’re transferring a 160KB file that represents 10 IO blocks of data so 10 blocks of 16 KB. And if you do that all in one second, that’s 10 credits in one second, so 10 IOPS

> When you aren’t using the volume much you aren’t using many IOPS and you aren’t using many credits

If you have *no credits* in this IO bucket, you *can’t perform any IO* on the disc

The IO bucket it has a capacity of 5.4 million IO credits and it fills at the baseline performance rate of the volume

⇒ Every volume has a baseline performance based on its size with a minimum

This means as an absolute minimum regardless of anything else you can consume 100 IO credits per second, which is 100 IOPS

The actual baseline rate, which you get with GP2 is based on the volume size.

You get three IO credits per second, per GB of volume size. This means that a 100 GB volume gets 300 IO credits per second, refilling the bucket. Anything below 33.33 recurring GB gets this 100 IO minimum. Anything above 33.33 recurring gets three times the size of the volume as baseline performance rate

By default GP2 can burst up to 3,000 IOPS. So you can do up to 3,000 input output operations of 16 KB in one second.

> If you’re consuming more IO credits than the rate at which you bucket is refilling then you’re depleting the bucket. If you’re consuming less than your base line performance, then you bucket is replenishing

You need to ensure that they’re staying replenished and not depleting down to zero

For volumes larger than one TB, they will have the baseline equal to or exceeding the burst rate of 3,000. And so they will always achieve their baseline performance as standard. They don’t use this credit system

The maximum IO per second for GP2 is currently 16,000. So any volumes above 5.33 recurring TB in size achieves this maximum rate constantly

> GP2 is great for boot volumes, for low latency interactive applications or for dev and test envs

#### GP3
![[EBS Volume Type - General-1.png]]
GP3 is also *SSD based*, but it *removes the credit bucket architecture of GP2* for something much simpler.

Every GP3 volume regardless of size starts with the *standard 3,000 IOPS*. So 3,016 KB operations per second and it can transfer 125MB per second.

That’s **standard regardless of volume size**.

If you need more performance then you can pay for up to 16,000 IOPS and up to 1,000 MB per second of throughput.

GP3 offers a higher max throughput as well, so you can get up to 1,000 MB per second versus the 250 MB per second maximum of GP2.

The usage scenarios for GP3 are also much the same as GP2: So the virtual desktops, medium-sized databases, low-latency applications, dev and testing Env and boot volumes..

You can safely swap GP2 to GP3 at any point, but just be aware that for anything above 3,000 IOPS the performance **doesn’t get added automatically** like with GP2, which scales on size.