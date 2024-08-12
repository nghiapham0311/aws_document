![[Route53 Multi Value Routing.png]]

Multi value routing in many ways is like a mixture between simple and fail-over taking the benefits of each and merging them into one routing policy.

With multi value routing, we start with a hosted zone and with multi value routing, you can actually create many records all with the same name. Each of those records in this example is an A record, which maps onto an IP address. Each of the records when using this routing type can have an associated health check.

When queried, up to 8 healthy records are returned to the client. If you have more than 8 records then 8 are selected at random.

At this point, client picks one of those values and uses it to connect to the resource. Because each of the records is health checked, any of the records, which fail the check, won’t be returned to the client and won’t be selected by the client when connecting to resources.

So multi value routing, it aims to improve availability by allowing a more active, active approach to DNS. You can use it if you have multiple resources, which can all service requests and you want to select one at random.

> **It’s not a substitute for a load balancer, which handles the actual connection process from a network perspective. But the ability to return multiple heal checkable IP addresses is a way to use DNS to improve availability of an application.**

So simple routing has no health checks and is generally used for a single resource such as a web server.

Fail-over is used for active backup architectures, commonly with an S3 bucket as a backup

Multi value is *used* when you have **many resources which can all service requests and you want them all health checked and then returned at random**. So any healthy records will be returned to the client. If you have more than 8, they’ll be returned randomly.

[[Route53 Weighted Routing]]