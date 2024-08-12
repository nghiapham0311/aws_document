![[ASG HealthCheck Comparison - EC2 vs ELB.png]]
ASGs assess the health of instances within that group using health checks. If an instance fails a health check, then it’s replaced within the ASG. So this is a method of automatically healing the instances within the ASG

There are **3** different types of health checks which can be used with ASGs.
- We have *EC2*, which is the default
- *ELB checks* which can be enabled on an ASG
- *Custom health checks*

With EC2 checks, any of these statuses is viewed as unhealthy. So essentially anything but the instance running is viewed as unhealthy. So if it’s stopping, if it stopped, terminated, if it shutting down or if it’s impaired, meaning it doesn’t have 2/2 status checks, then it’s viewed as unhealthy.

We also have the option of using LB a health checks, and for an instance to be viewed as healthy when this option is used the instance needs to be both running and it needs to be passing the LB a health check.

> This is important because if you’re using an ALB, then these checks could be application aware. So you can define a specific page of that application that can be used as a health check, you can do text pattern matching and this can be checked using an ALB. So when you integrate this with an ASG, the checks that that ASG is capable of performing become much more application aware

Finally, we have custom health checks, and this is where an external system can be integrated and mark instances as healthy or unhealthy. So this allows you to extend the functionality of these ASG health checks by implementing a process specific to your business or using an external tool

Health check **grace period**, so by default, this is **300 seconds or 5 minutes**, and **essentially this is a configurable value which needs to expire before health checks will take effect on a specific instance**. So in this particular case, if you select 300 seconds then it means that the system **has 5 minutes** to launch the system, to perform any bootstrapping, and then any application startup procedures or configuration **before it can fail a health check**.

So this is really useful if you’re performing bootstrapping with your EC2 instances which are launched by the ASG.

> **This is an important one because it does come up on the exam**

It’s often a cause of an ASG continuously provisioning and then terminating instances. *If you don’t have a sufficiently long health check grace period*, then you can be in a situation where the health checks start taking effect before the applications have finished configuring.

At that point, it will be viewed as unhealthy, terminated and a new instance will be provisioned. And that process will repeat over and over again. So you need to know how long your application instances take to launch, bootstrap, and then perform any configuration processes. And that’s how long you need to set your health check grace period to be.

[[SSL Offload & Session Stickiness & Gateway LB]]