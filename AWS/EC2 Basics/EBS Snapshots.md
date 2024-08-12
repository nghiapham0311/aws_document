![[EBS Snapshots-1.png]]

- New EBS Volume = full performance immediately
- **Snaps restore lazily** - fetch gradually
- Requested blocks are fetched immediately
- Force a read of all data immediately…
- Fast Snapshot Restore (FSR) - Immediate restore
- … up to **50** snaps per region. Set on the **Snap & AZ**

When you create a *new EBS volume without using a snapshot*, the performance is *available immediately*. There’s no need to do any form of initialization process

If you restore volume from a snapshot, it does the restore lazily.

If you attempt to *read* data which hasn’t been restored yet, it will *immediately pull it from S3*, but that achieves *lower levels of performance* than reading from EBS directly.

You can force a read of every block of the volume ⇒ done in the OS using tools ⇒ it forces EBS to pull all the snapshot data from S3 into that volume.

This is generally something that you *would do immediately* when you restore the volume, before moving that volume into production usage. It just *ensures perfect performance* as soon as your *customers start using that data*.

There’s a feature called **fast snapshot restore**, or FSP. This is an option that you can set on a snapshot which makes it instantly restore.

You can **create 50 of these fast snapshot restore per region**, and when you enable it on a snapshot, you *pick the snapshot specifically*, and the *AZs* that you want to be able to do instant restores to.

So one snapshot configured to restore to four AZs in a region, represents four out of that 50 limit of FSRs per region.

FSR actually *costs extra, it can get expensive*, especially if you have lots of different snapshots.

![[EBS Snapshots-2.png]]

#### Billing
![[EBS Snapshots.png]]

Snapshots are built using a GB month metric. So a 10 GB snapshot stored for one month represents 10GB month. A 20GB snapshot stored for half a month represents the same 10 GB month

This is used data, not allocated data. You might have the volume, which is 40 GB in size, but if you only use 10GB of that, then the first full snapshot is only 10GB.

EBS doesn’t charge for unused areas in volumes when performing snapshots. You’re charged for the full allocated size on EBS volume but that’s because it’s allocated for snapshots.

Because snapshots are incremental, you can perform them really regularly. Only the data that’s changed is store. So doing a snapshot every 5 mins won’t necessarily cost more than doing one per hour

[[EBS Encryption]]