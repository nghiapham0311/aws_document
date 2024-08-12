#### SSL Offload
3 different ways that ELB's can handle SSL
- SSL Bridging
- SSL Pass Through
- SSL Offloading
![[SSL Offload & Session Stickiness.png]]
#### Session Stickiness
![[SSL Offload & Session Stickiness-2.png]]
Users using this technique always redirect to EC2 instance.
They will be redirect to another server if the EC2 instance is down or the cookies is expired.
Causing un-even load between EC2 instance although we're using Load Balancer.
Need to implement stateless sessions or using another product like DynamoDB to store session.
#### Gateway LoadBalancer
![[SSL Offload & Session Stickiness & Gateway LB.png]]
Architect:
![[SSL Offload & Session Stickiness & Gateway LB-1.png]]

![[SSL Offload & Session Stickiness & Gateway LB-2.png]]

