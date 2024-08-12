A *private hosted zone* is a container that holds information about how you want Amazon Route 53 to respond to DNS queries for a domain and its subdomains **within one or more VPCs that you create with the Amazon VPC service**
- A **Public** Hosted Zone … **which isn’t public**
- Associated with **VPCs**
- … **only accessible** in those **VPCs**
- …Using **different accounts** is supported via **CLI/API**
- Split-view (overlapping **public** & **private**) for **PUBLIC** and **INTERNAL** use with the same zone name

A private hosted zone is just *like a public hosted zone* in terms of how it operates, only **it’s not public**. Instead of being public, it’s *associated with VPCs within AWS* and it’s *only* *accessible within VPCs that it’s associated with*.

It’s even possible to use a technique called split-view or split-horizon DNS which is where you have public and private hosted zones of the same name, meaning that you can have a different variant of a zone for private users vs public

![[R53 Private Hosted Zones.png]]
As with the public zones, we can create records within this zone. From the public internet, users can do normal DNS queries but the *private hosted zone* is **inaccessible from the public internet**.

It can be made accessible though from VPCs. So the VPC `+2` address. Any *VPCs* which we *associate with the private hosted zone*, will be **able to access** that zone via the resolver. Any VPCs which *aren’t associated* will face the same problem as the user on the public internet, *access isn’t available*.

So private hosted zones are great when you need to provide records using DNS, but maybe they’re sensitive and need to be accessible only from internal VPCs.

> Just remember to be able to *access* a private hosted zone, the *service* needs to be **running inside a VPC** and that **VPC needs to be associated with the private hosted zone**

#### R53 Split View
![[R53 Private Hosted Zones-1.png]]
You have a VPC running an Amazon Workspace and to support some business applications a private hosted zone with some records inside it. The private hosted zone is associated with VPC1 on the right meaning the workspace could use the R53 resolver to access the private hosted zone.

Now the private hosted zone is not accessible from the public internet but what split view allows us to do is to create a public hosted zone with the same name.

This public hosted zone might only have a subset of records that the private hosted zone house . So from the public internet, access to the public hosted zone would work in the same way as you would expect.

Any records inside the public hosted zone would be accessible, but records in the private hosted zone, which are not in the public hosted zone.

This is a common architecture where you want to use the same domain name for public access and internal access, but with a different set of records available to each

[[R53 CNAME vs ALIAS]]