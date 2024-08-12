Offer the quickest way to create a network link between AWS environment and something that's not AWS (on premise, another cloud provider or a data center).
#### Fundamentals
- A *logical connection* between a **VPC** and **on-premises network** encrypted using IPSec, running over the **public internet**.
- Site to Site VPN is fully *High Available* - if you design and implement it correctly.
- Quick to provision .... **less than an hour** (REMEMBER THIS FOR EXAM, as contract with long provisioning times for physical connection like *Direct Connect*).
- Component includes: VPC, Virtual Private Gateway (**VGW**) create and associated to a single VPC & can be target for a route table, Customer Gateway (**CGW**) either the logical configuration in AWS or the physical device that this logical configuration represent, VPN Connection between the **VGW** and **CGW**.
#### Architect
![[AWS Site-to-Site VPN.png]]
HA on AWS side, but not on customer site.

Create a new VGW and link to another CGW to go fully HA.
![[AWS Site-to-Site VPN-1.png]]
#### Static vs Dynamic VPN
Different is how route configuration.
Static VPN is simple but no load balancing and multi connection fail over.
![[AWS Site-to-Site VPN-2.png]]
#### VPN Considerations
- Speed limitations ~ **1.25Gbps**
- Latency Considerations - **inconsistent, public internet**
- Cost - AWS hourly cost, GB out cost, data cap
- Very quick to setup - **hours**
- Can be used as a backup for Direct Connect (**DX**)
- Can be used with Direct Connect (**DX**)

[[Direct Connect Concept]]