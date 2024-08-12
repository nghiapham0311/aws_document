#### VPC Router
- Every VPC has a VPC router - Highly available. Move traffics from somewhere to somewhere else. Its run in all AZs that VPC used.
- In every subnet .. ‘network + 1’ address is the address for *VPC Router*.
- Routes traffic between subnets.
- Controlled by ‘route tables’ each subnet has one.
- A VPC has a Main route table - subnet default. You can created a custom route table for subnet, then subnet will dis-associated with route table from VPC. *One subnets has only one route table, route table can be associated with many subnets.*
#### Route Table

![[VPC Rounting, Internet Gateway, Bastion Hosts.png]]

> I mentioned that 0.0.0.0/0 matches all IPv4 addresses, that’s known as a default route, a (indistinct)

These local routes can never be updated, they’re always present, and the local routes always take priority. They’re the exception to that previous rule about more specific the route is, the higher the priority.

**IMPORTANT**

**The route tables are attached to 0 or more subnets, a subnet has to have a route table. It’s either the main route table of the VPC or a custom one that you’ve created.**

A route table controls what happens to data as it leaves the subnet or subnets that that route table is associated with.

Local routes are always there, uneditable, and match the VPC IPv4 or v6 CIDR range.

For anything else, higher prefix values are more specific and they take priority. The way that a route works is it matches a destination IP and for that route, it directs traffic towards a specific target.
#### Internet Gateway (IGW)
- **Region resilient** gateway attached to a VPC
- 1 VPC = 0 or 1 IGW, 1 IGW = 0 or 1 VPC
- Runs from within the AWS Public Zone
- Gateways traffic between the VPC and the Internet / AWS Public Zone (S3..SQS..SNS..etc)
- Managed - AWS handles performance

One internet gateway will cover all of the AZs in the region which the VPC is using

There’s a one-to-one relationship between IGW and the VPC.

> A VPC can have no IGWs which makes it entirely private, or it can have one IGW.

![[VPC Rounting, Internet Gateway, Bastion Hosts-1.png]]

###### IPv4 Addresses with a IGW
![[VPC Rounting, Internet Gateway, Bastion Hosts-2.png]]

![[VPC Rounting, Internet Gateway, Bastion Hosts-3.png]]

The instance has a private IP address of let’s say 10.16.16.20 and it also has an IPv4 public address that’s assigned to it of 43.250.192.20 ⇒ Only, that’s not how it really works

**What actually happens with public IPv4 addresses is that they never touch the actual services inside a VPC.**

Instead, when you allocate a public IPv4 address, for example, to this EC2 instance, a record is created which the IGW. It links the instance’s private IP to its allocated public IP. ⇒ So the instance itself is not configured with that public IP

That’s why when you make EC2 instance and allocate it a public IPv4 address inside the operating system, it only sees the private IP address

Khi đi thi đừng có bị trick bởi câu hỏi assign IPv4 cho EC2 instance. EC2 instance chỉ biết mỗi private IP thôi, không biết một cái gì hết.
Nếu là IPv6 thì mặc định là public route nên EC2 instance có thể biết được public IP và cả private IP.

#### Bastion Host / Jumpbox
- Bastion Host = Jumpbox.
- An instance in a public subnet.
- Incoming management connections arrive there.
- Then access internal VPC resources.
- Often the only way IN to a VPC.

Đây là nơi mà mọi traffic khi muốn đi vào VPC phải đi qua. Nó như là cái cổng checkpoint của doanh trại bộ đội ấy, ai đi qua cũng phải xuất trình giấy tờ.
Cũng có thể xem như là một cái Hub.
Có thể config chỉ để cho một IP nhất định đi qua.

[[Stateful & Stateless firewall]]