It *removes* one more piece of *admin overhead*. The admin overhead of managing individual db instances. From now on when you referring to the Aurora product, you should refer to it as *Aurora provisioned* vs *Aurora serverless*
#### Aurora Serverless Concepts
![[Aurora Serverless.png]]
With Aurora Serverless, you don’t need to provision resources in the same way as you did with Aurora provisioned. You still create a cluster, but *Aurora Serverless* uses the concept of *ACUs or Aurora Capacity Units*.

*Capacity Units* represent a *certain amount of compute*, and a corresponding amount of memory. For a cluster, you *can set minimum and maximum values*. And Aurora Serverless will *scale* between *those values* adding or removing capacity based on the load placed on the cluster.

It can even *go down to 0 and be paused*, meaning that you’re only billed for the storage that the cluster consumes.

Billing is based on the resources that you use on a per second basis. and Aurora Serverless provides the same levels of resilience as you’re used to with Aurora provisioned. So you get cost to storage, that’s replicated across 6 storage nodes across multiple AZs.

ThIt removes much of the complexity of managing db instances and capacity. It’s easier to scale. It seamlessly scales the compute and memory capacity in the form of ACUs as needed with no disruption to client connections
#### Aurora Serverless - Use Cases
![[Aurora Serverless-1.png]]

[[Aurora Global Database]]
[[Database Migration Service (DMS)]]