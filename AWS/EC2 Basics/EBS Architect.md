- **Block** Storage - **raw** disk allocations (volume) - Can be **encrypted** using **KMS**.
- … instances see **block device** and create **file system** on this device (ext3/4, xfs).
- Storage is provisioned in **ONE AZ** (resilient in that **AZ**).
- Attached to **one** EC2 instance (or other service) over a storage network.
- … **detached** and **reattached**, not lifecycle linked to one instance … **persistent**.
- **Snapshot** (backup) into **S3**. Create volume from snapshot (migrate *between AZs*).
- Different physical storage types, different sizes, different performance profiles (SSD, high performance SSD, HDD, ...).
- Billed based on *GB-month* (and in some cases *performance*).

![[EBS Architect.png]]

You have an EC2 instance in AZ A. You might create an EBS volume within that same AZ and then attach that volume to the instance. Both of these are in the same AZ. You might have another instance which has two volumes attached to it

Over the time, you might choose to detach one of those volumes and then reattach it to another instance in the same AZ

That’s doable because EBS volumes are separate product with separate life cycles

> **You cannot communicate cross AZ with storage**

You can snapshot volumes to S3. And this means that the data is now replicated as part of that snapshot across AZs in that region. So that gives you additional resilience and it also gives you the ability to create an EBS volume in another AZ from this snapshot.

[[EBS Volume Type - General]]
[[EBS Volume Type - Provisioned IOPS]]
[[EBS - HHD Based]]
[[Choosing Instance Storage & EBS]]