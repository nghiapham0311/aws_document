Security Groups (SGs) are another security feature of AWS VPC ... only unlike NACLs they are attached to AWS resources, not VPC subnets.

SGs offer a few advantages vs NACLs in that they *can recognize AWS resources* and filter based on them, they can *reference other SGs* and also *themselves*.

But.. SGs are not capable of explicitly blocking traffic - so often require assistance from NACLs

- **STATEFUL** - detect response traffic automatically
- *Allowed* (IN or OUT) request = allowed response
- **NO EXPLICIT DENY** … only ALLOW or Implicit DENY
- … can’t block specific bad actors
- Supports IP/CIDR … and logical resources
- … including other security groups AND ITSELF
- Attached to ENI’s not instances (even if the UI shows it this way)

Even if you see the UI present this as being able to attach a SG to an instance, know that this isn’t what happens. When you attach a SG to an instance what it’s *actually doing is attaching the SG to the primary network interface* of that instance

⇒ SGs are attached to network interfaces.

![[Security Group.png]]

The SG applies to all traffic which enters or leaves the network interface.

SGs cannot explicitly block traffic. This means with this example if you’re allowing 0.0.0.0/0 to access the instance on port TCP, port 443 and this means the whole IPv4 internet then you *can’t block anything specific*.

If Bob was actually a bad actor. In this situation, *SGs cannot be use to add protection*. You can’t add an explicit deny for Bob’s IP address. That’s not something that SGs are capable of.
#### Logical References
![[Security Group-1.png]]
We could just add the IP address of the web instance into the SG at the application instance or if you wanted to allow our application to scale and change IPs, then we could add the CIDR ranges of the subnets instead of IP addresses ⇒ Possible but it’s not taking advantage of the extra functionality which SGs provide

What we could do is reference the web SG within the application SG.

Using a logical resource reference in this way means that the source reference, so the A4L-web SG. This actually references anything which has this SG associated with it

So in essence, this references this, so this logical reference within the application SG references the web SG and anything which has the SG attached to it.

Now this means we don’t have to worry about IP addresses or CIDR ranges and it also has another benefit. It scales really well.

When you *reference* a SG from another SG what you’re actually doing is *referencing any resources which have that SG associated with them*

This substantially *reduces the admin overhead* when you have multi-tiered applications. And it also simplifies security management which means it’s *prone to less errors*.
####  Self References
![[Security Group-2.png]]
The SG also has rule which is self referential rule, allowing all traffic

What this means is that if it’s attached to all of the instances, then anything with this SG attached can receive communications. So all traffic from this SG. And this effectively means anything that also has this SG attached to it

So it allows communications to occur, two instances which have it attached from instances which have it attached.

It handles any IP changes automatically, which is useful if these instances are within an auto scaling group, which has provisioning and terminating instances based on load on the system

It also allows for simplified management of any intra app communications such as MS Domain Controllers or managing application high availability within clusters

Cho phép traffic từ web thông qua TCP 1337 có thể lấy dữ liệu từ app layer.
Giữa app layer thì cho phép traffic được truyền tới 3 instance (High Availability).
Nếu scale lên cũng vậy, scale xuống cũng vậy, không cần phải thay đổi nhiều.

[[Network Address Translation (NAT) and NAT Gateways]]