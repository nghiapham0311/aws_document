It’s the *job* a *LB* to *accept connections from customers* and then to *distribute those connections across any registered backend compute*. It means that the user is **abstracted away** from the **physical infrastructure**. It *means* that the amount of *infrastructure can change so increase or decrease in number without affecting customers*.

Because the physical infrastructure is abstracted it means that infrastructure can fail and be repaired all of which is hidden from customers.
![[AWS/HA & Scaling/images/Elastic Load Balancer Architect.png]]
Let’s start off with a *VPC* which uses *2 AZ, AZ A and AZ B*. Then in those AZs we’ve got a few *subnets, 2 public and some private*. For this example, we’re going to assume that those services are long-running compute or EC2.

*ELBs* specifically Application Load Balancers, *support many different types of compute services, it’s not only EC2*. When you provision a LB you have to decide on a few important configuration items.

The first, you need to pick whether you want to use IPv4 only or dual Stack and dual Stack just means using IPv4 and the newer IPv6. You also need to pick the AZ which the LB will use. Specifically, you are picking one Subnet in 2 or more AZs.

> **This is really important, because this leads in to the architecture of ELBs. So how they actually work. Based on the Subnets that you picked inside AZs when you provision a LB the product places into these subnets one or more LB nodes.**

So what you see as a single *LB object* is actually *made up of multiple nodes and these nodes live within the Subnets that you pick*. So when you’re *provisioning* a LB you *need to select which AZs* it goes into, and the way you do this is by picking **one and one only Subnet in each of those AZs**.

In the example, I’ve picked to use the public Subnet in AZ A and AZ B. So the product has deployed one or more LB nodes into each of those Subnets.

When a *LB* is *created* it actually gets *created with a single DNS record*. It’s an A record. And this *A record actually points at all of the ELB nodes that get created with the product*. So any connections that are mode using the DNS name of the LB are actually made to the nodes of that LB. The DNS name resolve to all of the individual nodes. It means that any incoming requests are distributed equally across all of the nodes of the LB and these nodes are located in multiple AZs and they scale within that AZ. So they’re highly available. If one node fails, it’s replaced. If the incoming load to the LB increases then additional nodes are provisioned inside each of the Subnets that the LB is configured to use.

> **Another choice that you need to make when creating a LB, is to decide whether that LB should be internet-facing or whether it should be internal. This choice, so whether to use internet facing or internal controls the IP addressing for the load balancer nodes**

If you pick *internet-facing*, then the **nodes of the LB are given public addresses and private addresses**. If you pick *internal*, then the **node only have private IP addresses**. So that’s the only difference. Otherwise, they’re the same architecturally, the have the same nodes and the same LB features.

The connections from our customers, which arrive at the LB of nodes, the *configuration of how that’s handled* is done using a *listener configuration*. This configuration controls what the LB is listening to. So *what protocols and ports will be accepted at the listener or front side of the LB*.

Bob has initiated connections to the DNS name associated with the LB. That means that he’s made connections to LB nodes within our architecture. At this point, the LB nodes can then make connections to instances that are registered with this load balancer and the **load balancer doesn’t care about whether those instances are public/private** EC2 instances, so allocated with a public IP address or their private EC2 instances. So instances which reside in a private Subnet and only have private addressing.

> **Internet-facing LB that have public addresses, so it can be connected to from the public internet, it can connect both to public and private EC2 instances. Instances that are used do not have to be public.**

> **The importing is that if you want a LB to be reachable from the public internet, it has to be an internet-facing LB, because logically, it needs to be allocated with public addressing.**

LBs in order to function need 8 or more free IP addresses in the Subnets that they’re deployed into. Strictly speaking, this means a `/28` subnet which provides a total of 16 IP addresses but minus the 5 reserved by AWS, this leaves 11 free per Subnet*, but AWS suggests that you use a `/27` or larger Subnet to deploy an ELB in order that it can scale.

> **AWS do suggests in their documentation that you need a `/27` but they also say you need a minimum of 8 free IP addresses**

Internal LBs are architecturally just like internet-facing LBs except they only have private IPs allocated to their nodes. So internal LBs are generally used to separate different tiers of applications. In this example, our user Bob connects via internet-facing LB to the web server. Then this web server can connect to an application server via an internal LB. This allows us to separate application tiers and allow for independent scaling.
#### Decoupling 3 tier application
![[Elastic Load Balancer Architect 1.png]]
Now without LBs, *everything would be tied to everything else*. Our user Bob would have to communicate with a specific instance in the web tier. If this failed or scaled, then Bob’s experience would be disrupted. The instance that Bob is connected to would itself connect to a specific instance in the application tier. If that instance failed or scaled, then, again, Bob’s experience would be disrupted.

What we can do to *improve* this architecture is to put **LBs** *between the application tiers* to **abstract one tier from another**. How this changes things is that Bob actually communicates with an ELB node. This ELB node sends this connection through to a particular web server. But Bob has no knowledge of which web server he’s actually connected to because he’s communicating via a LB.

If *instances are added or removed*, then *he would be unaware of this fact* because he’s abstract away from the physical infrastructure by the LB. Now the web instance that Bob is using , it would need to communicate with an instance of the application tier, and it would do this via an internal LB. Again, this represents an abstraction of communication. So in this case, the web instance that Bob is connected to isn’t aware of the physical deployment of the application tier. It’s not aware of how many instances exist nor which one it’s actually communicating with. At this point, to complete this architecture, the application server that’s being used would use the db tier for any persistent data storage needs.

Without using LBs with this architecture, all the tiers are tightly coupled together. They need an awareness of each other. Bob would be connecting to a specific instance in the web tier. This would be connecting to a specific instance in the application tier. And all of these tiers would need to have an awareness of each other.

**LB remove some of this coupling**. They loosen the coupling and this allows the tiers to operate independently of each other because of this abstraction. Crucially, it allows the tiers to scale independently of each other.

For example, it means that if the load on the application tier increased beyond the ability of 2 instances to service that load, then the application tier could grow independently of anything else. In this case, scaling from 2 to 4 instances. This web tier could continue using it with no disruption or reconfiguration because it’s abstracted away from the physical layout of this tier. Because it’s communicating via a LB, it has no awareness of what’s happening within the application tier.
#### Cross-Zone LB
![[Elastic Load Balancer Architect-1.png]]
We know now that a LB by default has at least one node per AZ that it’s configured for. In this case, an application load balancer will have a minimum of 2 nodes, one in each AZ. The DNS name for the LB will direct any incoming requests equally across all of the nodes at the LB.

In this example, we have 2 nodes, one in each AZ. Each of these nodes will receive a portion of incoming requests based on how many nodes there are. For 2 nodes, it means that each node gets 100% divide by 2, which represents 50% of the load that’s directed at each of the LB nodes.

In production situations, you might have more AZs being used and at higher volume, so higher throughtput, you might have more nodes in each AZ.

So however much incoming load is directed at the LB DNS name, each of the LB nodes will receive 50% of that load.

Now originally, LB were restricted in terms of how they could distribute the connections that they received. Initially, the way that it worked is that each LB node could only distribute connections to instances within the same AZ.

This might sound logical, but consider this architecture where we have 4 instances in AZ A and one instance in AZ B. This would mean that the LB node in AZ A would split its incoming connections across all instances in that AZ, which is 4 ways and the node in AZ B would also split its connections up between all the instances in the same AZ. But because there’s only one, that would mean 100% of its connections to the single EC2 instance.

With this historic limitation, it means that node A would get 50% of the overall connections and would further split this down 4 ways, which means each instance would be allocated 12.5% of the overall load. Node B would also receive 50% of the overall load. Normally, it would split that down across all instances also in that same AZ. But because there’s only one, that one instance would get 100% of that 50%. So all of the instances in AZ A would receive 12.5% of the overall load, and the instance in AZ B would receive 50% of the overall load. ⇒ So this represents a substantially uneven distribution of incoming load because of this historic limitation of how LB nodes could distribute traffic.

The fix for that was a feature known as cross-zone LB. It simply allows every LB node to distribute any connections that it receives equally across all registered instances in all AZs.

So in this case, it would mean that the node in AZ A could distribute connections to the instance in AZ B and the node in AZ B could distribute connections to instances in AZ A and this represents a much more even distribution of incoming load.

This is a *feature* which originally was **not enable by default** but if *you’re deploying an ALB, this comes enabled as standard*
## Exam powerup
![[Elastic Load Balancer Architect-2.png]]

Firstly, when you provision an ELB, you see it as one device which runs in 2 or more AZs. Specifically, one subnet in each of those AZs.

But what you’re actually creating is one ELB node in one subnet in each AZ that the LB is configured in.

You’re also creating a DNS record for that LB, which spreads the incoming requests over all of the active nodes for that LB. Now you start with a certain number of nodes, let’s say 1 node per AZ but it will scale automatically if additional load is placed on that LB.

Remember, by default, cross-zone LB means that nodes can distribute requests across to other AZs, but historically, this was disabled, meaning connections, potentially, would be relative imbalanced. But for ALBs, cross-zone load balancing is enable by default

LB come in 2 types. Internet-facing, which means that the node are allocated with public IPv4 addresses. It doesn’t change where the LB is placed. It just influences the IP addressing for the nodes of that LB. Internal LBs are the same, only their nodes are only allocated private IP addresses.

> **An internet-facing LB can communicate with public instances or private instances. EC2 instances don’t need public IP addressing to work with an internet-facing LB. An internet-facing LB has public IP addresses on its nodes. It can accept connections from the public internet and balance these across both public and private EC2 instances**

LBs are configured via listener configuration, controls what those LBs listen to.

They require 8 or more free IP addresses per subnet that they get deployed into. The AWS documentation suggests a `/27` in order to allow scaling.

[[ALB vs NLB]]