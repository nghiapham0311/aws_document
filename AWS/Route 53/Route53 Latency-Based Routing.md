![[Route53 Latency-Based Routing.png]]
**Latency-based routing should be used when you’re trying to optimize for performance and user experience, when you want Route53 to return records which can provide better performance.**

It starts with a hosted zone within R53 and some records with the same name. In addition, for each of the records, you can specify a record region. So `use-east-1`, `us-west-1` and `ap-southeast-2` in this example.

Latency-based routing supports *one record with the same name for each AWS region*. The idea is that you’re specifying the region where the infrastructure for that record is located.

In the background, AWS maintains a database of latencies between different regions of the world. So when a user makes a resolution request, it will know that that user in Australia in this example. It does this by using an IP lookup service. And because it has a database of latencies, it will know that a user in Australia will have a certain latency to `us-east-1`, `us-west-1` , and hopefully the lowest latency to a record which is tagged to be in the Asia Pacific region, so `ap-southeast-2` . So that record is selected and it’s returned to the user and used to connect to resources.

Latency-based routing can also be *combined with health checks*. If a record is unhealthy then the next lowest latency is returned to the client making the resolution request.

This type of routing policy is designed to **improve performance for global applications by directing traffic towards infrastructure with the best, so lowest latency, for users accessing that application.**

It’s worth noting though that the **database** which **AWS maintain isn’t real time**. It’s updated in the background and doesn’t really account for any local networking issues, but it’s better than nothing and can significantly help with performance of your applications.

[[Route53 Geolocation Routing]]