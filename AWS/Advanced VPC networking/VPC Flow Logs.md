- Capture package metadata (source IP, destination IP, source port, destination port,...) (**Not package content**)
- Attached to a **VPC** - All ENIs in that VPC.
- Attached to **Subnets** - All ENIs in that Subnet.
- Attached to **ENI** directly.
- Flow log aren't **real-time**.
- Log destination *S3* or *CloudWatchLog*.
#### FlowLogs Architect
![[VPC Flow Logs.png]]

[[Egress Only Internet Gateway]]