![[Auto-Scaling Groups.png]]
*ASG* do one thing they *provide Auto-Scaling for EC2*. Strictly speaking they can also be used to implement a self-healing architecture as part of that scaling or in isolation.

*ASGs* make *use of configuration defined within launch template or launch configurations* and that’s how they know what to provision

An ASG uses one launch configuration or one specific version of a launch template, which is linked to it. You can change which of those is associated but it’s one of them at a time. So all instances launched using the ASG are based on the single configuration definition either defined inside a specific version of a launch template or within a launch configuration

ASG has **3** super important values associated with it, we’ve got the **minimum size, the desired capacity and the maximum size**. For example, 1:2:4 means 1 minimum, 2 desired and 4 maximum.

ASG has one foundational job which it performs, it keeps the number of running EC2 instances the same as the desired capacity and it does this by provisioning or terminating instances. So the **desired capacity always has to be more than the minimum size and less than the maximum size**. If you have a desired capacity of 2 but only one running EC2 instance than the ASG provisions a new instance. If you have a desired capacity of 2 but to have 3 running EC2 instances then the ASG will terminate an instance to make these 2 values match.

You *can keep an ASG entirely manual*, so there’s no automation and no intelligence. you just update values and the ASG performs the necessary scaling actions. Normally though **scaling policies** are you was together with ASG.

**Scaling policies** can *update the desired capacity based on certain criteria for example, CPU load* and if the desired capacity is updated, it will provision or terminate instances.
![[Auto-Scaling Groups-1.png]]
On the *ASG* we specify a minimum value in this case one and this means there will always be at least one running EC2 instance in this case. We can also set to desired capacity in this example too and this will add another instance if a desired capacity is set which is higher than the current number of instances if this is the case then instances are added

Finally, we could set the maximum size in this case to 4, which means that 2 additional instances could to be provisioned but they won’t immediately be because the desired capacity is only set to 2 and there are currently 2 running instances. We could manually adjust the desired capacity up or down to add or remove instances which would automatically be built based on the launch template or launch configuration.

Alternatively, we could **use scaling policies to automate** that process and **scale in or out based on sets of criteria**.
#### Auto Scaling Groups Architecture
![[Auto-Scaling Groups-2.png]]
Architecturally *ASGs* **define where instances are launched**, they’re *linked to a VPC and subnets within that VPC are configured on the ASG, whatever subnets are configured will be used to provision instances into*.

When instances are provisioned there’s an attempt to keep the number of instances within each AZ even. So in this case, if the ASG was configured with 3 subnets and the desired capacity was also set to 3 then it’s probable each subnet would have one EC2 instance running within it but this isn’t always the case.

The ASG will try and level capacity where available. Scaling policies are essentially rules, rules which you define which can adjust the values of an ASG and there are 3 ways that you can scale ASGs
#### Scaling Policies
![[Auto-Scaling Groups-3.png]]
The *first* is not really a policy at all it’s just to *use Manual Scaling*. This is where you *manually adjust the value* as at any time and the *ASG handles any provisioning or termination* that’s required.

**Scaling Policies is optional, ASG can have no Scaling Policies**

Next the *Scheduled Scaling* which is great for *sale periods* where you can scale out the group when you know there’s going to be additional demand or when you know a system won’t be used so you can scale in outside of business hours.

*Scheduled Scaling* *adjusts the desired capacity* **based on schedules** and this is useful for **any known periods of high or low usage**.

> For the exam if you have **known periods of usage** then **Scheduled Scaling** is going to be a *great potential answer*.

Then we have *Dynamic Scaling* and there are **3** *sub types*, what they all have in common is they are rules which react to something and change the values on an ASG.

- The first is *Simple Scaling* and this well it’s simple. This is most commonly a pair of rules one to provision instances and one to terminate instances. You define a rule based on their metric and an example of this is CPU utilization. If the metric, for example, CPU utilization is above 50% then adjust the desired capacity by adding one. And if the metric is below 50% then remove one from the desired capacity. Using this method you can scale out meaning adding instances or scale in meaning terminating instances based on the value of a metric. This metric isn’t limited to CPU it can be many or the metrics including memory or disk input output. So metrics need the CloudWatch Agents to be installed you can also use some metric not on the EC2 instances. For example, maybe the length of an SQS Queue or a custom performance metric within your application such as response time.
- We also have *Stepped Scaling* which is similar but you *define more detailed rules*. This allows you to act depending on how out of normal the metric value is. So maybe at one instance, if the CPU usage is above 50% but if you have a sudden spike of load maybe add 3 if it’s above 80% and the same could happen in reverse. *Stepped Scaling* allows you to react quicker the more extreme the changing conditions. *Stepped Scaling* **is almost always preferable to simple** *except when your only priority is simplicity*.
- Lastly, we have *target tracking* and this takes a slightly different approach. It lets you define an ideal amount of something, say 40% aggregate CPU and then the group will scale as required to stay at that level provisioning or terminating instances to maintain that desired amount or that target amount. *Not all metrics work for target tracking*, but some examples of ones that are supported are *average CPU utilization, average network in, average network out* and the one that’s relevant to ALBs request count per target.

Stepped Scaling CPU 50% -60% scale 1.
70%-90% scale 2.
90% - 100% scale 3.
This is called _step adjustments_.
Simple scaling just 50+ then scale, < 50 then decrease.

Lastly, there’s a configuration on an ASG called a *Cooldown Period* and this is a *value in seconds*. It controls **how long to wait at the end of a scaling action before doing another**. It allows ASGs to wait and review chaotic changes to a metric and can avoid costs associated with constantly adding or removing instances because remember there is a minimum billable period. Since you’re billed for at least the minimum time every time an instance is provisioned regardless of how long you use it for.
#### Health
![[Auto-Scaling Groups-4.png]]ASGs also monitor the health of instances that they provisioned by **default this uses the EC2 status checks**. So if an EC2 instance fails, EC2 detect this passes this on to the ASG and then the ASG terminate the EC2 instance then it provisions a new EC2 instance in it’s place this is known as self healing, and it will fix most problem isolated to a single instance.

The same would happen if we terminated an instance manually the ASG would simply replace it.

There’s a trick with EC2 and ASG if you create a launch template which can automatically build an instance then create an ASG using that template set the ASG to use multiple subnets in different AZs then set the ASG to use a minimum of one maximum of one and a desired of one then you have simple instance recovery.

This instance will recover if it’s terminated or if it fails and because ASGs work across AZs, the instance can be re-provisioned in another AZ if the original one fails, it’s cheap, simple, and effective HA.
#### ASG + Load Balancers
![[Auto-Scaling Groups-5.png]]
Take this example that Bob is browsing to the cat blog that we’ve been using so far and he’s now connecting through a LB and the LB has a listener configured for the blog and points at a target group.

Instead of statically adding instances or other resources to the target group then you can use an ASG configured to integrate with the target group as instances are provisioned within the ASG then they’re automatically added to the target group of that low balancer.

Then as instances are terminated by the ASG then they’re removed from that target group. This is an example of elasticity because metrics which measure load on a system can be used to adjust the number of instances. These instances are effectively added as LB targets and any users of the application because they access via the LB are abstracted away from the individual instances and they can use the capacity added in a very fluid way.

*ASG* can be **configured to use the LB health checks rather than EC2 status checks**. ALB checks can be much richer they can monitor the state of HTTP or HTTPS requests. Because of this, their application aware which simple status checks which EC2 provides or not.

> Be careful though, you need to use an appropriate LB health check. If your application has some complex logic within it and you’re only testing a static HTML page then the health check could respond as okay, even though the application might be in a failed state. And the inverse of this if your application uses dbs and your health check checks a page with some db access requirements well, if the db fails then all of your health checks could fail meaning all of your EC2 instances will be terminated and re-provisioned when the problem is with the db not the instances. ⇒ So you have to be **really careful when it comes to setting up health checks**
#### Final points
![[Auto-Scaling Groups-6.png]]
*ASG* are **free** the only **costs are for the resources created by the ASG**. And to avoid excessive costs use cool downs within the ASG to avoid rapid scaling.

To be cost effective you should also think about using more smaller instances because this means you have more granular control over the amount of compute and therefore costs that are incurred by your ASG.

So if you have 2 larger instances and you need to add one, that’s gonna cost you a lot more than if you have 20 smaller instances and only need to add one.

Smaller instances, mean more granularity which means you can adjust the amount of compute in smaller steps and that makes it a more cost-effective solution

ASG are used together with ALBs for elasticity. So the LB provides the level of abstraction away from the instances provisioned by the ASG, so together they use to provision elastic architectures.

Lastly, an ASG controls the when and the where. So when instances are launched and which subnets they’re launched into. Launch templates or launch configurations define the what, so what instances are launched and what configuration those instances have.

[[ASG Lifecycle Hooks]]


