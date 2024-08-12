- Do the **same** job as the VPC Endpoint Gateway. But how to do is different.
- Use for AWS Service **except** S3 and DynamoDB... But S3 is now supported.
- VPC Endpoint Interface are not Highly Available **by default**. Added to *specific subnets* - an *ENI* - not **HA**.
- For **HA** .. add **one endpoint**, to **one subnet**, **per AZ** used in the VPC.
- Network access controlled via **Security Group**.
- **Endpoint Policies** - restrict what can be done with that endpoint.
- **TCP** and **IPv4** only.
- Behind the scene using PrivateLink.
- Using private DNS not like Gateway using Route Table and Prefix List.

Without using VPC Endpoint VPC:
![[VPC Endpoint (Interface).png]]
Using VPC Endpoint VPC:
![[VPC Endpoint (Interface)-1.png]]

[[VPC Peering]]