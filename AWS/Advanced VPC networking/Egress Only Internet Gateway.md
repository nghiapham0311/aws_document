Its an Internet Gateway which only allow connection to be initiated from inside VPC to outside.

- With IPv4 addresses are private or public
- **NAT** allows **private IPs** to access public internet. **Without allowing** externally initiated connection (**IN**), NAT doesn't work with IPv6
- With IPv6 all IPs are public.
- Internet Gateway (IPv6) allows all IPs **IN** and **OUT**
- Egress-Only is **outbound-only for IPv6**, not receive any connection from outside to go into.

![[Egress Only Internet Gateway.png]]
[[VPC Endpoint (Gateway)]]