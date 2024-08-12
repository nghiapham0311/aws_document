![[EBS - HHD Based.png]]

EBS Provides a number of different volume types

- Throughput Optimized HDD — A low-cost HDD designed for frequently accessed, throughput-intensive workloads.
- Cold HDD — The lowest-cost HDD design for less frequently accessed workloads

**st1** is designed for **when cost is a concern but you need frequent access storage for throughput intensive sequential workloads**. So things like big data, data warehouses and log processing

**sc1** one the other hand is designed for in **frequent workloads**. It’s geared towards a maximum economy when you just want to store lots of data and **don’t care about performance.** This storage type is the lowest cost EBS storage available. It’s designed for less frequently accessed workloads. So if you have colder data, archives, or anything which requires less than a few loads or scans per day, then this is the type of storage volume to pick

> If you have a requirement for anything IOPS based, then avoid of these and look at SSD based storage