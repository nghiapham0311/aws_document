![[Elastic Load Balancer Version.png]]
Elastic Load Balancer refers to all of current load balancer (CLB, ALB, NLB).

The load balancers are *split* between **version one and version two**. You should **avoid using the version one** load balancer at this point and aim to migrate off them onto v2 products, which should be preferred for any new deployments.

The load balancer product started with the **classic load balancer known as CLB**, which is the only v1 load balancer. This was introduced in 2009. It’s one of the older AWS products.

**Classic load balancers can load balance HTTP and HTTPS**, as well as lower level protocols, but they aren’t really Layer 7 devices. They don’t really understand HTTP and they can’t make decisions based on HTTP protocol features. They lack much of the advanced functionality of the v2 load balancers, and they can be significantly more expensive to use.

One common limitation is that *classic load balancers only support one SSL certificate per load balancer*, which means for larger deployments, you might need hundreds or thousands of classic load balancers, and these could be consolidated down to a single v2 load balancer

With **v2 load balancers**, the **first is the application load balancer or ALB** and these are truly Layer 7 devices, so application layer devices. They *support HTTP, HTTPS and the web socket protocols*. They’re **generally the type of load balancer that you’d pick for any scenarios which use any of these protocols**.

There’s also **network load balancers or NLBs**, which are also **v2 devices**, but these s**upport TCP, TLS which is a secure form of TCP and UDP protocols**. So NLBs are the type of load balancer that you would **pick for any applications which don’t use HTTP or HTTPS.**

For example, if you wanted to load balance email servers or SSH servers or game which used a custom protocol, so didn’t use HTTP or HTTPS, then you would use a NLB.

In general, v2 load balancers are faster and support target groups and rules, which allow you to use a single load balancer for multiple things or handle the load balancing different based on which customers are using it.

[[Elastic Load Balancer Architect]]