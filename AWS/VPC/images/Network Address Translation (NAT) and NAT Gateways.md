#### What is NAT?
- Network Address Translation (NAT)
- A set of processes - remapping SRC or DST IPs
- **IP masquerading** - hiding CIDR Blocks *behind one IP*
- Public IPv4 Addresses running out
- Gives Private CIDR range **outgoing** internet* access

IP masquerading or what we’ll refer to as NAT gives a whole private range of IP addresses outgoing, only access to the public internet and the AWS public zone.
#### NAT Architect
![[Network Address Translation (NAT) and NAT Gateways.png]]

NAT offers us a third option and it works really well in this style of situation.

We provision a NAT gateway into a public subnet. And remember the public subnet allows us to use public IP addresses the public subnet has a route table attached to it which provides default IPv4 routes pointing at the internet gateway

Because NAT gateway is located in this public web subnet, it has a public IP which is routable across the public internet

So it’s now able to send data out and get data back in return

The private subnet where instances are located can also have its own route table. This route table can be different than the public subnet route table ⇒ We could configure it so that the route table that’s on the application subnet has a default IPv4 route.

But this time, instead of pointing at the IGW, like the web subnet users we configure this private route table so that it points at the NAT Gateway.

This means when those instances are sending any data to any IP addresses that do not belong inside the VPC by default, this default route will be used and that traffic will get sent to the NAT gateway.
##### Packet flows

Because we have this default route on the route table of the application subnet that packet is routed through to the NAT Gateway.

The NAT Gateway makes a record of the data packet. It stores the destination that the packet is for, the source address at the instance sending it and all the details which help it identify the specific communication in future

Remember multiple instances can be communicating at once. And for each instance, it could be having multiple conversations with different public internet hosts

So the NAT gateway needs to be able to uniquely identify those. And so it records the IP addresses involved, the source and destination, the port numbers, everything it needs into a translation table

NAT gateway maintain something called a translation table which records all of this information

Then it adjusts the packet, the one that’s been sent by the instance, and it changes the source address of this IP packet to be its own source address

Remember nothing inside of VPC really has directly attached to it, a public IPv4 address.

So the packet is moved from the gateway to the IGW by the VPC router. At this point, the IGW knows that this packet is from the NAT Gateway. It knows that the NAT Gateway has a public IPv4 address associated with it and so it modifies the packet to have a source address of the NAT gateways public address and it sends it on its way.

The NAT gateways job is to allow multiple private IP addresses to masquerade behind the IP address that is has
#### NAT Gateways
- Runs from a **public subnet**
- Uses **Elastic IPs** (Static IPv4 Public)
- **AZ resilient Service** (HA in that AZ) - High Availability
- For region resilience - **NATGW in each AZ …**
- … RT (Route table) in for each AZ with that NATGW as target
- Managed, scales to 45 Gbps, $ Duration & Data Volume
#### VPC Design - NATGW Full Resilience
![[Network Address Translation (NAT) and NAT Gateways-1.png]]

You need private route tables in each AZ. Each of these would need to have their own route table, which would have a default IPv4 route which points at the NAT gateway in the same AZ.

That way, if any AZ fails, the others could continue operating without issues.

**IMPORTANT**

THIS IS **FALSE** ⇒ a NAT gateway is truly regionally resilient

A NAT gateway is *highly available in the AZ that it’s in*. So if hardware fails or it needs to scale to cope with load, it can do so in that AZ.

But if the whole AZ fails, there is no fail-over. You provision a NAT gateway into a specific AZ, not the region. It’s not like the IGW, which by default is region resilient

For NAT gateway, you have to deploy one into each AZ that you use if you need that region resilience.
#### NAT Instance vs NAT Gateway
![[Network Address Translation (NAT) and NAT Gateways-2.png]]

*NAT instance* just the NAT process running on an *EC2 instance*.

If an instance is running as a NAT instance, then it will be receiving some data which the source address will be of other resources in that VPC and the destination will be a host on the internet

So it will neither be the source nor the destination. So by default, that traffic will be dropped.

If you need to allow an EC2 instance to function as a NAT instance, then you need to *disable a feature called source and destination checks*. This can be disabled via the console UI, the CLI and the API.

NAT instances and NAT gateways are kind of the same, they both need a public IP address, they both need to run in a public subnet and they both need a functional IGW

It’s *not really preferred to use EC2 running as a NAT instance*. It’s much easier to use a NAT gateway, and it’s recommended by AWS in most situations

There are a few key scenarios where you might want to consider using an EC2 based NAT instance.

- If you value availability, bandwidth, low levels of maintenance, and high performance, then you should choose NAT gateways.
- A NAT gateway offers high end performance, it scales, it’s custom designed to perform network address translation. A NAT instance in comparison is limited by the capabilities of the instance it’s running on.
- A NAT instance is also general purpose, so it won’t offer the same level of custom designed performance as NAT gateway.
- Now availability is another important consideration. A NAT instance is a single EC2 instance running inside an availability zone. It will fail if its storage fails or if its networking fails, and it will fail if the AZ itself fails entirely.

A NAT gateway has some benefits over a NAT instance. So inside one AZ, it’s highly available so it can automatically recover. It can automatically scale. So it removes almost of the risk of outage vs a NAT instance.

But it will still fail entirely if the AZ fails entirely. ⇒ You still need to provision multiple NAT gateways spread across all the AZs that you intend to use if you want to ensure complete availability.

For maximum availability, a NAT gateway in every AZ you use.

**IMPORTANT**

Now if *cost* is your primary choice then a *NAT instance can be cheaper*. It can also be significantly cheaper at high volumes of data.

You can use a very small EC2 instance, even ones that are free tier eligible to reduce cost. And the instances can also be fixed in size meaning they offer predictable costs

A NAT gateway will *scale automatically*, and you’re *billed for both the NAT gateway and the amount of data transferred*, which increases as the gateway scales. A NAT gateway is also not free tier eligible

NAT instances also offer other niche advantages. Because they’re just EC2 instance, you can connect to them just like you would any other EC2 instance

- You can multipurpose them so you can use them for other things such as bastion hosts
- You can also use them for port-forwarding ⇒ you can have a port on the instance externally that can be connected to over the public internet and have this forwarded onto an instance inside the VPC
- You can be completely flexible when you use NAT instance

With a NAT gateway, this isn’t possible because you don’t have access to manage it. It’s a managed service.

A NAT gateway *cannot be used as a bastion host*. It **cannot do port-forwarding because you cannot connect to its operating system**

*NAT instances* are just EC2 instances. And so you *can filter traffic using either network ACLs on the subnet the instance is in or security groups directly associated with that instance*.

*NAT gateways don’t support security groups*. You can only use knuckles with NAT gateways.
#### What about IPv6?
- NAT isn’t required for IPv6
- All IPv6 addresses in AWS are publicly routable
- The IGW works with ALL IPv6 IPs directly
- NAT Gateways **don’t work with IPv6**
- ::/0 Route + IGW for bi-directional connectivity
- ::/0 Route + Egress-Only IGW - Outbound Only