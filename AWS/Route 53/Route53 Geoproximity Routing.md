![[Route53 Geoproximity Routing.png]]

Geoproximity routing lets Amazon Route 53 route traffic to your resources **based on the geographic location of your users and your resources**. You can also *optionally choose to route more traffic or less* to a given resource by specifying a *value*, known as a *bias*. A bias expands or shrinks the size of the geographic region from which traffic is routed to a resource.

Geoproximity **aims to provide records which are as close to your customers as possible**. If you recall, latency based routing provides the record which has the lowest estimated latency between your customer and the region that the record is in. Geoproximity aims to **calculate the distance between a customer and the record** and answer with **the lowest distance**.

**Geoproximity routing lets R53 route traffic to your resources based on the geographic location of your users and your resources but you can optionally choose to route more traffic or less traffic to a given resource by specifying a value. The value is called a bias.**

**A bias expands or shrinks the size of a geographic region that is used for traffic to be routed to**
[[Route53 Interoperability]]
