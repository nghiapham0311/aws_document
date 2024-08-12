#### Fundamentals
![[Transit Gateway.png]]

*The Transit Gateway* is a **network transit hub** which **connect** *VPCs to each other and to on-premises network using site-to-site VPN and Direct Connect*.

Design to reduce network complexity in AWS.

It's a network gateway object. Like other gateway object, it's *Highly Available and Scalable*.

The architecture is that you create **attachments** which is how Transit Gateway connects to other network on AWS & It's how it connects to on-premises networks.

Currently available **attachments** is *VPC, Site-to-Site, Direct Connect Gateway*.
#### Problem with network connect
![[Transit Gateway-1.png]]

Assume we have 4 VPCs and a company office. Requirement is to connect all VPCs together and connect with company office's network.

Because VPCs peering don't support transitive connect. So in order to connect all 4 VPCs we need 6 VPC peering.

To connect from 4 VPCs to company office's network. We're using VPN, 2 for each VPCs. So total 8.

But we need to ensure the Highly Availability, we need to add one more Customer Gateway to company office's network.

So total 16 connects.
Super complex and lots of admin overhead. This architecture scales badly, assume we have more sites, more VPCs :(
#### Transit Gateway
![[Transit Gateway-2.png]]

## Exam Powerup
- Transit Gateway supports transitive routing.
- Can be used to create global networks (peering with another Transit Gateway in same region or cross account).
- Less complexity.

[[Storage Gateway]]