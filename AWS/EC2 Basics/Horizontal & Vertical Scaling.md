Within AWS Horizontal and Vertical scaling are two ways which systems have to deal with *increasing or decreasing user-side load*.

Each has pros and cons but handles the act of scaling radically differently.
#### Vertical Scaling
![[Horizontal & Vertical Scaling.png]]
Vertical scaling is one way of achieving this increase in capacity so this increase of resource allocation

The instance will service a certain level of incoming load from our customers, but at some point, assuming the load keeps increasing, the size of this *instance will be unable to cope*, and the experience for our customers will begin to *decrease*.

Customers might experience *delays*, *unreliability*, or even *outright system crashes*

We might pick a t3.extra large, which doubles the virtual CPU and memory. Or if the rate of increase is significant, we could go even further and pick another size up.

When you’re actually performing *vertical scaling with EC2*, you’re actually **resizing** an EC2 instance when you scale. And because of that **there’s downtime, often a restart during the resize process**, which can potentially cause customer **disruption**.

But it goes beyond this. Because of this disruption it *means* that you can only *scale generally during pre-agreed times*, so within outage windows.

If incoming load on a system changes rapidly, then this restriction of only being able to scale during outage windows limits how quickly you can react, how quickly you can respond to these changes by scaling up or down

As load increase on a system you can scale up but larger instances often carry a price premium. ⇒ The increasing **cost** going **larger** and **larger** is often not linear towards the top end.

Because you’re scaling individual instances, there’s always going to be an upper cap on performance. This cap is the maximum instance size. size của EC2 là có hạn thì chính cái size bự nhất là limit của cách scaling này.

> With vertical scaling, this will always be the cap on the scaling of an individual compute resource

- Each resize requires a reboot - **disruption**
- Larger instances often carry a **$ premium**
- There is an upper cap on performance - **instance size**
- **No application modification** required
- Works for ALL applications - **even Monoliths**
#### Horizontal Scaling
![[Horizontal & Vertical Scaling-1.png]]
Horizontal scaling is designed to address some of the issues with vertical scaling

Horizontal scaling is still designed to cope with changes to incoming load on a system, but instead of increasing the size of an individual instance, *horizontal scaling* just **adds more instances**.

As the load increases on a system, horizontal scaling just adds additional capacity ⇒ They all need to work together, all need to take their share of incoming load placed on the system by customer. This generally means some form of load balancer.

A *load balancer* is an *appliance* which sits between your servers, in this case instances and your customers. When customers attempt to access the system, all that incoming load is distributed across all of the instances running your application. Each instance gets a fair amount of the load.

When you think about horizontal scaling, **sessions are everything**

With horizontal scaling, you can be shifting between instances constantly. That’s one of the benefits. It evens out the load.

> → Horizontal scaling needs either application support or what’s known as **off-host sessions**. If you use off-host sessions then your *session data* is stored in another place, an *external database* → This means that the servers are what’s called *stateless*. They’re just dumb instances of your application. Your application doesn’t care which instance you connect to because your session is externally hosted somewhere else.

- Sessions, Sessions, Sessions
- Requires application support OR **off-host sessions**
- **No disruption** when scaling
- **No real limits** to scaling
- Often less expensive - **no large instance premium**
- More granular…
## Exam Powerup
Horizontal vs Vertical Scaling
![[Horizontal & Vertical Scaling-2.png]]

[[Instance Metadata]]