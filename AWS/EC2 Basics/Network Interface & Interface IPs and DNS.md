![[Network Interface & Interface IPs and DNS.png]]

Every EC2 instance has *at least one* which is the *primary* interface or primary ENI (*Elastic Network Interface*)

*Optionally*, you can attach *one or more secondary ENI*, which can be in separate *subnets*, but everything **needs to be within the same** availability zone

EC2 is isolated in one AZ.

When you’re interacting with the instance from a networking perspective, you’re often seeing elements of the primary network interface.

> Those *security groups* are actually on the *network interface*. Not the instance

Each interface also has a **primary IP v4** address that from *the range* of the *subnet* that the *interface is created in*.

You can have *zero or more secondary* private IP addresses also on the interface

You can have *zero or one public IP addresses* associated with the interface itself.

You can also have *one elastic IP* address *per private IPv4 address*. Elastic IP addresses are public IPv4 addresses - and different than normal public IPv4 addresses where it’s one per instance.

You can have *zero or more IPv4 addresses* per interface

> *IPv6* by default, *publicly routable*. With IPv6, there’s no definition of public or private addresses, they’re all public addresses

You can have *security groups*, these are applied to *network interfaces*.

> A SG that’s applied on a particular interface will impact all IP addresses on that interface.

Per interface, you can also **enable or disable the source and destination check**. This means that if traffic is on the interface, it’s going to be discarded if it’s not from one of the IP addresses on the interface as a source or destined to one of the IP addresses on the interface as a destination.
## Exam Power Up
- If you *assign* Elastic IP to EC2 instance then the public **IPv4 will be remove**.
- If you *un-assign* Elastic IP to EC2 instance the the public **IPv4 will be assign** and this time it's **new** IP, not the same as before.
- Secondary ENI + MAC = **Licensing**.
- Multi-homed (subnets) Management & Data.
- Different Security Groups - **multiple interfaces**.
- OS - **Doesn't see public IPv4**.
- IPv4 Public IPs are **Dynamic** … Stop & Start = **Change**.
- Inside VPC Public DNS resolve = **private IP in VPC**, public IP everywhere else.
[[Amazon AMI]]
