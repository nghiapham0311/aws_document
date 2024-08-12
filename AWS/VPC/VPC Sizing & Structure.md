Thứ cần quyết định đầu tiên đó chính là dải IPs mà VPC sử dụng (VPC CIDR)
Là một trong những thứ quan trọng nhất cần phải quyết định trước để design solution vì không dễ dàng để sửa đổi sau này. Gây ra rất nhiều painful sau này.

Một số câu hỏi cần ghi nhớ khi design:
- What size should the VPC be..
- Are there any Networks we can’t use …
- VPC’s, Cloud, On-premises, Partners & Vendors
- Try to predict the future
- VPC Structure - Tiers & Resiliency (Availability) Zones. Ví dụ như application tier, database tier, mình càng chia nhỏ ra thì càng dễ phân phát IPs và quản lý.
Nếu nghi ngờ cái gì thì hãy nghĩ tới trường hợp xấu nhất.

#### Real world project
Những dải IPs dưới đây đã được occupied rồi, ta cần phải tránh những dải IPs này khi design solution cho animal4life organization.

![[VPC Sizing & Structure 1.png]]


> [!WARNING] Cách chia IPs
> IPs 192.168.10.0/24 số 24 cuối có nghĩa là nó occupied 3 số đầu của IPs, mỗi số 8 bit, 3 số 24bit. Dãy này có range từ 192.168.10.0 -> 192.168.10.255.
> 
> IPs 192.168.10.0/16 số 16 cuối có nghĩa là nó occupied 2 số đầu của IPs, mỗi số 8 bit, 2 số 16bit. Dãy này có range từ 192.168.10.0 -> 192.168.255.255.
> 
> IPs 192.168.10.0/9 số 9 cuối có nghĩa là nó occupied 1 số đầu của IPs. Dãy này có range từ 192.168.10.0 -> 192.255.255.255.

#### More considerations
- VPC minimum /28 (16 IP), maximum /16 (65536 IPs)
- Personal preference for the 10.x.y.z range
- Avoid common ranges - avoid future issues
- Reserve 2+ networks per region being used per account
- 3 US, Europe, Australia. Tổng (5) x2 = 10 ranges - Assume 4 Accounts
- ⇒ Total 40 ranges (ideally) cho 4 account.

To summarize where we are:
- We’re going to use the 10 range
- We’re going to avoid 10.0 to 10.10 because they’re far too common.
- We’re going to start at 10.16, because that’s a nice clean base two number
- We **can’t use 10.128 through to 10.255**, because potentially that’s used by Google Cloud

⇒ So that give us a range of possibilities from 10.16 to 10.127 inclusive which we can use to create our network

#### VPC Sizing
![[VPC Sizing & Structure 1.png]]
A subnet is located in one Availability Zone.

The first decision point that you need to think about is how many Availability Zones your VPC will use

Step 1: Pick how many Availability Zones your VPC will use

> Usually start with three as the default. Because it will work in almost any region. But I also always add a spare, because we all know at some point things grow so aim for at least one spare

This mean as a minimum for Availability Zone: A, B, C *and the spare*

That means that we have to at least split the VPC into at least four smaller networks. So if we started with a /16, we would now have four /18s

As well as the AZs inside of VPC, we also have tiers, and tiers are for different types of infrastructure that are running inside that VPC.

We might have a web tier, an application tier, a database tier that makes three and you should always add buffer.

> So my default is to start with four tiers. Web, application, database and a *spare*

Now the tiers used in your architecture might be different, but my default for most designs is to assume three, plus a spare.

If you only used one AZ, then each tier would need its own subnet, meaning four subnets in total, but we also have four AZs. And since we want to take full advantage of the resiliency provided by these AZs, we need the same base networking duplicated in each AZ.

So each tier has its own subnet in each AZ, four web subnets, four app subnets, four database subnets and four spares. ⇒ a total of 16 subnet

If we chose a /16 for the VPC, that would mean that each of the 16subnets would need to fit into that /16. So a /16 VPC split into 16 subnets results in 16 smaller network ranges, each of which is a /20
#### Proposal
![[VPC Sizing & Structure-1.png]]
[[Custom VPCs]]