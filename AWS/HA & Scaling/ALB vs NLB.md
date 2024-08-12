#### Consolidation
![[ALB vs NLB.png]]
Historically when using classic LBs, you connected instances directly to the LB or you integrated an Auto Scaling group directly with that LB, an architecture which looked something like this. So a single domain name, [catagram.io](http://catagram.io), using a single classic LB, and this has attached a single SSL certificate for that domain. And then an ASG is attached to that, and the classic LB distributes connections over those instances.

The problem is that this doesn’t scale because classic LBs don’t support SNI and you can’t have multiple SSL certificates per LB. So every single unique HTTPS application that you have requires its own classic LB. This is one of the many reasons that classic LBs should be avoided.

With this example, we have catagram and doggogram and both of these are HTTPS applications, and the only way to use these is to have 2 different classic LBs.

Compare this to the same application architecture. Both of these applications, only this time using a single application LB. So this is handling both applications. This time, we can use listener-based rules, but each of these listener-based rules can have an SSL certificate handling HTTPS for both domain. Then we can have host-based rules which direct incoming connections at multiple target groups which forward these on to multiple ASGs. This is a 2 to 1 consolidation.

Moving from v1 to v2 offers significant advantages. One of those is consolidation.
#### Application Load Balancer (ALB)
![[ALB vs NLB-1.png]]
It’s a true *Layer 7 LB*, and it’s *configured* to *listen* on either *HTTP or HTTPS* protocol. So these are layer 7 application protocols. An ALB understands both of these and can interpret information carried using both of those protocols.

The flip side to this is that the ALB *can’t understand any other Layer 7 protocols*. So things such as SMTP, SSSH or any custom gaming protocols are not supported by a Layer 7 LB such as the ALB.

The *ALB* has to *listen using HTTP or HTTPS listeners*. It **cannot** be configured to directly **listen using TCP, UDP or TLS** and that does have some important limitations

Because it’s a layer 7 LB, it can *understand Layer 7 content*, any *cookies* which are used by your application, custom *headers*, *user location and application behavior*. the Layer 7 LB, so the ALB is able to inspect all of the Layer 7 application protocol information and make decisions based on that information.

An important consideration about the ALB is that any inkling connection so HTTP or HTTPs, and remember HTTPS is just HTTP which is transiting using SSL or TLS, in all of these cases, whichever type of connection is used, it’s terminated on the ALB. **This means that you can’t have an unbroken SSL connection from your customer through to your application instances. It’s always terminated on the LB and then a new connection is made from the LB through to the application.**

It can’t do E2E unbroken SSL encryption between a client and your application instances. It also means that all ALBs which use HTTPS must have SSL certificates installed on that LB because the connection has to be terminated on the LB and then a new connection made to the instance

*ALBs* are also **slower** then NLB *because* there are *additional levels of the networking stack which need to be processed*. So the more levels of the networking stack which are involved, the more complexity, the slower the processing.

> If you’re facing any **exam questions** which are really strict on **performance** then you might **want to look at NLBs rather than ALBs**

A benefit though that ALBs offer is because they’re layer 7 then they *can evaluate the application health at Layer 7*. So in addition to just testing for a successful network connection, they can actually make an application layer request to the application to ensure that it’s functioning correctly.
#### Application Load Balancer (ALB) - Rules
![[ALB vs NLB-2.png]]
If you make a connection to a LB, *what the LB does* with that connection is *determined by any* **rule**. so rules **are** *processed in priority order*. You can have many rules which might affect a given set of traffic and they’re processed in priority order.

The *last one* to be processed is the *default rule* which is a **`catchall`**. But you can add additional rules and each of these can have conditions.

Now things that you can have *inside the conditions of a rule* include checking for things like *host-headers, http-headers, http-request-methods, path-patterns, query-strings and even source-ip*. So these rules could take different actions depending on which domain name you’re asking for.

They can perform different actions based on which path you’re looking for, so images or API. They can even take different decisions based on query-string and even make different decisions based on the source-ip address of any customers connecting to that ALB.

**Rules** *can also have actions*. There are things that the rules do with the traffic.

So they can **forward that traffic through to a target group, they can redirect traffic at something else, so maybe another domain name**. **They can provide a fixed HTTP response, a certain error code or a certain success code. They can even perform certain types of authentication, so using OpenID or using Cognito.**
![[ALB vs NLB-3.png]]
We’ve got one host-based rule with an attached SSL certificate, and the rule is using host header as a condition and forward as an action. So it’s forwarding any connections for [catagram.io](http://catagram.io) to the target group for the catagram application. But what if you want additional functionality?

Let’s imagine that we want to use the same ALB for a corporate client who’s trying to access [catagram.io](http://catagram.io). We can easily handle that by defining a listener rule. This time the condition will be the source IP address. This rule would have an action to forward traffic to separate target group, an ASG, which handles a second set of instances dedicated for this corporate client.

The HTTP connection from our enterprise users are terminated on the LB, and there’s a separate set of connections through to our application instances. There’s no option to pass through the encrypted connection to the instances, it has to be terminated.

> If you have to forward encrypted connections through to the instances without terminating them on the LB, then you need to use a **NLB**.

You could route based on paths or anything else in HTTP protocol, such as headers. Let’s say that this ALB was also handling traffic for doggogram. You could define a rule which matched the [doggogram.io](http://doggogram.io) domain name, and as an action, instead of forwarding, you could configure a redirect towards [catagram.io](http://catagram.io).
#### Network Load Balancer
![[ALB vs NLB-4.png]]
*NLBs* function at **Layer 4** so they’re a Layer 4 device. Which means that they can interpret TCP, TLS and UDP protocols as well as TCP and UDP.

The flip side of this is that they have *no visibility or understanding of HTTP or HTTPS*. This means that they can’t interpret headers, they can’t see or interpret cookies, and they’ve got no concept of session **stickiness** from a HTTP perspective.

NLBs are *really, really, really fast, they can handle millions of requests per second and have around 25% of the latency of ALBs*. This is because they don’t have to deal with any of the computationally of the networking stack. They only have to deal with Layer 4.

This also means that they’re ideal to deal with any non HTTP or HTTPS protocols. Examples might be SMTP email, SSH, game servers which don’t use either of the web protocols, and any financial applications which are not HTTP or HTTPS.

One of the *downsides* of not being aware of Layer 7 is that health checks which are performed by **NLB only check ICMP and basic TCP handshaking**. So they’re not application aware. You can’t do detailed health checking with NLBs.

A **benefit of NLBs is that it can be allocated with static IP address which is really useful for whitelisting if you have any corporate clients**. So corporate clients can decide to whitelist the IPs of NLBs and allow them to progress straight through their firewall.

Another benefit is that they *can forward TCP straight through to instances*. Because the NLB doesn’t understand HTTP or HTTPS then you can configure a listener to accept TCP-only traffic and then forward that through to instances. What that means is that any of the layers that are built on top of TCP are not terminated on the load balancer. so they’re not interrupted. This means you can forward unbroken channels of encryption directly from your clients through to your application instances.

NLBs are also used for private link to provide services to other VPCs.
#### Choosing
![[ALB vs NLB-5.png]]
1. If you want to perform unbroken encryption between a client and your instances, then use NLBs.
2. If you need to use static IPs for whitelisting, then again, NLBs
3. If you want the absolute best performance, so millions of requests per second and low latency, then again, NLBs.
4. If you need to operate on protocols which are not HTTP or not HTTPS, then you need to use NLBs
5. If you have any requirements which involves private link, then you need to use NLBs.
6. For anything else, default to using ALBs
[[Launch Configuration vs Launch Template]]
