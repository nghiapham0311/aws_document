Placement groups *allow* you to influence *placement*. Ensuring that instances are **either physically close together or not**

Types:
- **Cluster** - Pack instances close together
- **Spread** - Keep instances separated
- **Partition** - groups of instances spread apart
#### Cluster Placement Groups
![[EC2 Placement Group.png]]

> So *cluster placement groups* are used when you really need **performance** that needed to achieve **the highest levels of throughput and the lowest consistent latencies** within AWS but the *trade off* is because of the physical location (same rack). If the hardware that they’re running on *fails* logically it could *take down all of the instances*

- Can’t span AZs - **ONE AZ ONLY** - locked when launching first instance
- Can span VPC peers - but impacts performance
- Requires a supported instance type
- Use the same type of instance (**not mandatory**)
- Launch at the same time (**not mandatory … very recommended**)
- **10Gbps single stream performance**
- Use cases: **Performance, fast speeds, low latency**
#### Spread Placement Groups
![[EC2 Placement Group-1.png]]
**These are designed to ensure the maximum amount of availability and resilience for an application**

- Provides infrastructure isolation
- …each **INSTANCE** runs from a different rack
- Each rack has its own **network** and **power** source
- **7 Instances per AZ** (HARD LIMIT)
- Not supported for Dedicated Instances or Hosts
- **Use Case**: Small number of critical instances that need to be kept separated from each other
#### Partition Placement Groups
![[EC2 Placement Group-2.png]]

The key to understanding the difference is that partition placement groups are designed for **huge scale parallel processing systems** where you **need to create groupings of instances and have them separated**. You as the designer of a system can have control over which instances are in the same and different partitions. So you can design your own resilient architecture.

- **7 Partitions per AZ**
- Instances can be placed in **a specific partition**
- … or auto placed
- Great for topology aware applications
- …HDFS, HBase and Cassandra
- Contain the impact of failure to part of an application
[[Enhanced Networking & EBS Optimized]]