#### Fundamentals
- IPSEC is a group of protocols
- It sets up <font color="#d99694">secure tunnels</font> across *insecure networks*. Between <font color="#d99694">two peers</font> (<font color="#4f81bd">local</font> and *remote*) across the internet.
- You might use this if your business with multiple sites, spread around geographically and want to connect them together.
- Or you have infrastructure in AWS and another cloud provider and want to connect to that infrastructure.
- Provide <font color="#d99694">Authentication</font> so that peers which are known to each other and can authenticate with each other can connect.
- Data traffic carried by IPSec is <font color="#d99694">encrypted</font>.
#### Architect
![[IPSec VPN Fundamentals.png]]


Two type of VPNs:
- Route-based VPNs
- Policy-based VPNs
The different is how they match interesting traffic (traffic get sent over VPN)

[[AWS Site-to-Site VPN]]