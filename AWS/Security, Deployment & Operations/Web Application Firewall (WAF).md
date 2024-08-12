*WAF* is **AWS's implementation of L7 firewall**. Firewall capable of understand L7 protocol (HTTP, HTTPS).
#### WAF Architect
WAF is the product, but the actual unit of configuration in the product is the WEB ACL.
You can associate WEB ACL with **CloudFront, ALB, API Gateway**. Need to config this when create WEB ACL. Create in the Region.
If you want to access the log from WAF quicky, should not use S3 because log will be deliver every 5 minutes.
![[Web Application Firewall (WAF).png]]
#### WEB ACL
- Control what traffic is allow or block.
- Start with default action (ALLOW or BLOCK) - Non matching.
- Resource Type - CloudFront or Regional Service.
- ... ALB, API GW, AppSync .. pick region.
- Add Rule Groups or Rules processed in order.
- Web ACL Capacity Unit - Default 1500. can be increased via support ticket.
- WEB ACL are associated with resources (this can take time). Adjusting takes less time than associate one.
Resources can have one ACL. one WEB ACL can associated with many resources.
WEB ACL cant be used with AWS Output.
#### Rule Groups
- Rule Groups contains Rules.
- They don't have default actions ... that's defined when groups or rules are added to WEBACLs.
- Managed (AWS or Marketplace), Yours, Service Owned (i.e Shield & Firewall Manager).
- Rule Groups can be reused in multiple WEBACL.
- Default maximum in Rule Groups is 1500.
#### Rules
- Structure: Type, Statement, Action.
- One of two *types*: Regular or Rate-based (something occurs as a rate).
- *Statement*: **What** to match. e.g Incoming TCP port 80, 5000 connections in 5 minutes. Support origin country, IP, header, cookies, query parameter, query string, body (**first 8192 bytes ONLY**), HTTP method.
- *Action*: Allow, Block, Count, Captcha...
#### Pricing
- per WEBACL - Monthly 5$/month.
- RULE on WEBACL - Monthly 1$/month.
- REQUESTS per WEBACL - Monthly 0.6$/1 mil.
- Captcha - 0.4$/1000 challenge attempts.

[[AWS Shield]]