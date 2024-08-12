- Provide **private access** to *supported AWS Service* such as **S3** and **DynamoDB**. This service is in public zone and so normally, public IP address and configuration are required to connect with those services. *VPC Endpoint Gateway* **provide access** to those services **without** required any public IPv4 and configuration.
- Create per service, per region.
- **Prefix List** added to route table => **Gateway Endpoint** is the target.
- Highly Available (**HA**) across all AZs in **region** by default. Not going into any particular AZ.
- Endpoint Policy is used to control what it can access. Using to restrict access to particular S3 Bucket not the entire S3.
- Regional... **can't access cross region** services.
- VPC Endpoint Gateway can only access from inside that VPC.
- Can be use to created private read only S3 Bucket.

Using NATGW and Internet Gateway
![[VPC Endpoint (Gateway)-1.png]]

Using VPC Endpoint Gateway Architect
![[VPC Endpoint (Gateway).png]]

[[VPC Endpoint (Interface)]]