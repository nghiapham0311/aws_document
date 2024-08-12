![[Custom VPCs.png]]
- Regional Service - All AZs in the regions.
- Isolated network.
- Nothing *IN* or *Out* without explicit configuration.
- Default VPC is being setup by AWS, requiring no configurations.
- Flexible configuration - simple or multi-tier.
- Hybrid networking - other cloud & on-premises.
- Default or Dedicated Tenancy (*Chạy phần cứng riêng cho VPC, không khuyến khích trừ khi biết rõ mình đang cần gì, cứ stick với default hardware đi cho đỡ đau đầu*).
- VPC can use IPv4 private CIDR block & Public IPs.
- VPC will have 1 primary Private IPv4 CIDR Block.
- This block will have the min size of /28 (16 IP) and max /16 (65536 IP)
- We can have an optional secondary IPv4 Blocks (*need to raise ticket, and maximum 5 of them can be used*).
- Can be configured to use IPv6 /56 CIDR Block (this feature is not mature).
#### DNS in a VPC
- Provided by *Route53*
- VPC `Base IP + 2` Address (include enableDnsHostnames & enableDnsSupport) (*Ví dụ VPC là 10.0.0.0 thì DNS là 10.0.0.2*)
- **enableDnsHostnames** - gives instances DNS Names.
- **enableDnsSupport** - enables DNS resolution in VPC.
Cần check 2 settings màu đỏ nếu có vấn đề liên quan tới *VPC*.

Khi tạo VPC nhớ vào VPC Settings để bật 2 options này lên, thường thì option đầu đã được enable by default, nhớ vô bật option số 2 lên.
[[VPC Subnets]]