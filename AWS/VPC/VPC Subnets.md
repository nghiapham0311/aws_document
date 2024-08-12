#### Subnet
Subnets are what service run from inside VPCs.
In AWS diagram, Blue means private subnet, Green means public subnet.
Subnets inside VPC start with private by default and it takes some configurations to make it public.

- Subnet is AZ resilient.
- A sub network of a VPC - *within a particular  AZ*. Created with in on AZ and never be changed. If AZ failed, then subnet running inside that AZs failed and so on any service hosted on that subnets. => Design subnets within multiple AZs to solve this problem.
- 1 Subnet -> 1 AZ. But 1 AZ can have *Zero or Multiple subnets within it*.
- By default using IPv4 for networking, and it's allocated an IPv4 CIDR. This CIDR is a subset of VPC CIDR and has to be in the range of VPC CIDR.
- CIDR cannot overlap with other subnets in VPC.
- Can optionally use IPv6 and IPv6 CIDR if VPC is enable for IPv6.
- Subnets inside one VPC can communicate with another subnets as long as they are inside same VPC.
#### Subnet IP Addressing
- Reserved IP addresses (5 in total)
- 10.16.16.0/20 (10.16.16.0 ⇒ 10.16.31.255)
- **Network** Address (10.16.16.0) - *This is the first address of the network, this cant be use.*
- ‘Network + 1’ (10.16.16.1) - *The first address plus one is used by VPC Router.*
- ‘Network + 2’ (10.16.16.2) - *This address is used for Reserved (DNS).*
- ‘Network + 3’ (10.16.16.3) - *Reserved Future Use.*
- **Broadcast** Address 10.16.31.255 (Last IP in subnet).

A VPC has a configuration object apply to it called a DHCP Option Set.
> DHCP stands for dynamic host configuration protocol. It’s how computing devices receive IP addresses automatically

There’s one DHCP options set applied to a VPC at one time and this configuration flows through to subnets

It controls things like DNS servers, NTP servers, net bios servers and a few other things

For every VPC, there’s a DHCP option set that’s linked to it, and that can’t be changed. You can create option sets, but you cannot edit them.

⇒ If you want to change the setting you need to create a new one and then change the VPC allocation to this new one

On every subnet, you can also define two important IP allocation options.

- The first option control if resources in a subnet are allocated a public IPv4 address in addition to that private subnet address automatically
- Another option is whether resources deployed into that subnet are also given an IPv6 address and logically for that to work, the subnet has to have an allocation as does the VPC

But both of these options are defined at a subnet level and flow onto any resources inside that subnet.

[[VPC Routing, Internet Gateway, Bastion Hosts]]