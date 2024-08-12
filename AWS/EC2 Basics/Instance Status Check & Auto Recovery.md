![[Instance Status Check & Auto Recovery.png]]

Every instance within EC2 has two high level per Instance status checks.

When you initially launch an Instance, you might see these listed as initializing and then you might see only one of two passing. But eventually, all Instances should move into the two out of two past checks which indicate that all is well within the Instance

Each of the two checks represents and so a failure of either of them suggests a different set of underlying problems

The *first status* check is the *system status*. The *second* is the *Instance status*

There are manual activities that you could perform but EC2 comes with a feature allowing you to recover automatically to any system check issues.

You can ask the EC2 stop a failed Instance, reboot it, terminate it, or you can ask EC2 to perform Auto Recovery

> Auto Recovery *moves* the Instance to *a new host starts it up* with exactly the *same configuration as before*

→ All the **IP addressing is maintained**. If software on the instance is set to Auto start, this process could mean that the Instance, automatically recovers fully from any failed status check issues

Importantly, it would need to be in the same *AZ*. It will only take action for an isolated failure either the Host or the Instance

This feature does rely on having spare EC2 host capacity. → In case of major failure in multiple AZ in a region, there is a potential that this won’t work if there is not spare capacity

You also need to be using modern types of Instances so things like A1, C4, C5, M4, M5, R3, R4, R5

This feature *won’t work* if you’re using *Instance Store Volume*, it’ll only work on Instances which solely have EBS volumes attached

It’s a simple way that EC2 adds some automation which can attempt to recover an Instance and avoid waking up a CIDR admin.

> It’s not designed to automatically recover against large scale or complex system issue.

It’s a very simple feature which answers a very narrow set of error based scenarios. It’s not something that’s gonna fix every problem in EC2

> Termination Protection is a feature which adds an attribute to EC2 instances meaning they cannot be terminated while the flag is enabled.
> 
> It provides protection against unintended termination and also allows role separation, where junior admins can be allowed to terminate but ONLY for instances with no protection attribute set.

[[Horizontal & Vertical Scaling]]