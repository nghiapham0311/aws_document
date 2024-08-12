Enhanced networking is the AWS implementation of *SR-IOV*, a standard **allowing a physical host network card to present many logical devices which can be directly utilized by instances**.

This means lower host CPU *usage*, *better throughput*, *lower and consistent latency*

*EBS optimization* on instances means **dedicated bandwidth for storage networking - separate from data networking.**

![[Enhanced Networking & EBS Optimized.png]]

## Exam Powerup

- **EBS** = Block storage over the **network**
- Historically network was **shared** .. **data** and **EBS**
- EBS Optimized means **dedicated capacity** for EBS
- Most instances **support** and have **enabled by default**
- Some support, but enabling costs extra