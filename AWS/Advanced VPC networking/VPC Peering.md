- VPC Peering let's you created *private* and *encrypted network between two VPCs*.
- One VPC Peering link 2 VPCs and **no more than 2**.
- Connection between 2 VPCs can be in the *same/cross region* or *same/cross account*.
- **(optional) Public Hostnames** resolve to **private IPs**.
- **Same region SG's** can reference **peer SGs**. Only work if peer are inside same region.
- VPC Peering does NOT support transitive peering (A -> B, B -> C => A -> C is not gonna happened).
- Routing configuration is needed, SG and NALs filter.
![[VPC Peering.png]]
