- AWS Shield *Standard* & *Advanced* - provided **DDOS Protection**.
- Shield *Standard* is **free**, Shield *Advanced* has a **cost**.
#### Standard
- Free for all AWS customers.
- Protection at the perimeter. It can be at region/VPC or the AWS edge.
- Protect again Common Network (L3) or Transport (L4) layer attacks.
- Best protection using *R53, CloudFront, AWS Global Accelerator*.
#### Advanced
- 3000$ per month (per ORG), **1 year lock-in + data (OUT)/m**.
- Protects CF, R53, Global Accelerator, Anything Associated with EIPs (i.e EC2), ALBs, CLBs, NLBs.
- **Not automatic** - must be enable explicitly.

[[CloudHSM]]