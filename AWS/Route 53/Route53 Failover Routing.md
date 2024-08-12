![[Route53 Failover Routing.png]]
With *failover routing*, we start with a hosted zone and inside this hosted zone www record. However, with *failover routing*, we can **add multiple records of the same name**; a primary and a secondary. Each **of these records points at a resource.**

A common example is an out of band failure architecture where you have a primary application endpoint, such as an EC2 instance and a backup or fail over resource using a different service such as an S3 bucket.

They key element of *failover routing* is the inclusion of a health check. The *health check generally occurs on the primary record*. If the primary record is healthy then any queries to www, in this case, resolve to the value of the primary record which is the EC2 instance

If the primary record *fails it’s health check*, then the *secondary* value of the same name is *returned*. In this case, the S3 bucket. The use case for failover routing is simple

Use it when you need to **configure active passive failover where you want to route traffic to a resource when that resource is healthy, or to a different resource, when the original resource is failing its health check**.

[[Route53 Multi Value Routing]]