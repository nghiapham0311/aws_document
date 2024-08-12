
Storage Gateway can be used for a hybrid cloud setup, where data is produced on premise and synced to aws.
![[Storage Gateway - Volume-2.png]]
#### Fundamentals
![[Storage Gateway - Volume.png]]

Storage Gateway normally runs as a virtual machine on premises.

Storage Gateway act as a bridge between storage that exists on-premises and AWS.

On premises side, presents storage using NFS (Linux), SMB (Windows).
On AWS side, it integrates with EBS, S3 and Glacier.

Used for Migration data from on-premises to AWS, Extensions data center to AWS, running low storage on-premises and want to store in AWS,...

#### Type and How To Choose
![[Storage Gateway - Volume-3.png]]
##### File Gateway
File Gateway supports a file interface into S3 and combines a service and a virtual software appliance.

The software appliance, or gateway, is deployed into your on-premises environment as a virtual machine running on VMWare ESXI or Microsoft Hyper-V hypervisor.

File Gateway supports:
- S3 Standard.
- S3 Standard - Infrequent Access.
- S3 One Zone - Infrequent Access.

With File Gateway you can do the following things:
- You can store and retrieve files using NFS.
- You can store and retrieve files using SMB.
- You can access your data directly in S3 and from any AWS Services or cloud application.
- You can manage your S3 data using lifecycle policies, cross-region replication and versioning.

File Gateway now supports S3 Object Lock, enabling Write-Once-Read-Many (WORM).

Any modifications such as files edit, deletes or rename from the gateway's NFS or SMB clients are stored a new versions of the objects, without delete the previous versions.

File Gateway local cache can supports up to 64TB of data.
##### Volume Gateway
Volume Gateway provides cloud-backed storage volume that you can mounted as iSCCI devices from your on-premises application servers.

There are 2 types:
`Cache Volumes`: you store your data in S3 and retain a copy of frequently accessed data subsets locally. Cached volumes can range from 1GiB to 32TiB in size and must be rounded to the nearest GiB. Each Gateway configured for cached volumes can support up to 32 volumes.
![[Storage Gateway - Volume-4.png]]

`Stored Volume`: If you need low latency access to your entire data set, first configure your on-premises gateway to store all your data locally. Then asynchronous backup point-in-time snapshots of this data to S3. Stored Volumes can range from 1GiB to 16TiB in size and must be rounded to nearest GiB. Each Gateway configured to support 32 volumes.
![[Storage Gateway - Volume-5.png]]

Customers using the Volume Gateway configuration for block storage can detach and attach volume from and to a Volume Gateway. You can use this feature to migrate volumes between gateway to refresh underlying server hardware, switch between virtual machine types, and move volume to better host platform or newer version of EC2.
##### Tape Gateway
Tape Gateway archive backup data in Amazon Glacier.

Tape Gateway has a virtual tape library (VTL) interface to store data on virtual tape and cartridges that you created.

Deploy your gateway into EC2 instances to provision iSCCI storage volume in AWS.

Tape Gateway integrates with AWS S3 Glacier Deep Archive storage class, allowing you to store virtual tapes in the lowest-cost S3 storage class.

Tape Gateway also has the capability to move your virtual tapes archived in Amazon S3 Glacier to Amazon S3 Glacier Deep Archive. Enabling you to further reduce the monthly cost to store long-term data in the cloud up to 75%.

Support Write-Once-Read-Many and Tape Retention Lock on virtual tapes.
![[Storage Gateway - Volume-6.png]]
[[AWS Directory]]