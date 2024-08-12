![[ASG Lifecycle Hooks.png]]
So *lifecycle hooks* allow you to *configure custom actions* which can *occur* **during ASG actions**. So you can define actions which occur either during instance launch transitions or instance terminate transitions.

This allows you to do is when an *ASG scales out or scales in*, it will either launch or terminate instances. Normally, this process is completely under the control of ASG. So as soon as it makes a decision to provision or terminate an instance, this process happens with no ability for you to influence the outcome.

What lifecycle hooks do is when you create them instances are paused within the launch or terminate flow, and they pause or wait in this state until one of 2 things happen.

Either a **configurable timeout** and when that timeout expires, which by default is **`3600`** **seconds**, they will either continue or abandon the ASG action.

The alternative is whatever process that you perform, you can explicitly resume the process using **CompleteLifecycleAction** once you’ve performed whichever activity you want to perform.

Lifecycle hooks can either be integrated with EventBridge or SNS Notifications, which allow your systems to perform event driven processing based on the launch or termination of EC2 instances within an ASG
![[ASG Lifecycle Hooks-1.png]]
Normally when an ASG gets a scale out situation, an instance will be launched and it starts off in the Pending state. When it completes it will move into the InService state, but this gives us no opportunity to perform any custom activities

What we could do is define a lifecycle hook and hook into the instance launch transition. So if we do hook into this transition, the instance would move from Pending to Pending: Wait, and it would wait in this state

This allows us to perform a set of custom actions. An example might be to load or index some data which might take some time. And during this time, the instance stays in this state. Once done, it will move from a Pending: Wait state to a Pending: Proceed state, and from there it would move into the InService state

This is the process when configuring a lifecycle hook for this part of an EC2 instance’s lifecycle

The same happens in reverse if we define an instance terminate hook. What would normally happen when a scaling event happens would be the instance would move from a Terminating state to a Terminated state.

What we could do is define a lifecycle hook to hook into that, instead, the instance would move from Terminating to Terminating: Wait, where it would wait for a timeout. By default this is 3600 seconds and it would wait at this point, or *until we run the CompleteLifecycleAction operation*.

We could use this time period to maybe backup some data or logs or otherwise tidy up the instance prior to its termination. Once the timeout expired, or when we explicitly call CompleteLifecycleAction, then it would move from Terminating: Wait to Terminating: Proceed, and then finally through to the Terminated state.

Lifecycle hooks can integrate with SNS for transition notifications and EventBridge can also be used to initiate other processes based on the hooks in an event driven way.

[[ASG HealthCheck Comparison - EC2 vs ELB]]