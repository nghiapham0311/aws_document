#### Some problems with Lambda
![[AWS Step Function.png]]
A Lambda function cannot run past this 15 minute limit for its execution duration. You can in theory, chain Lambda functions together

Each env is isolated, cleaned each time and any data needs to be transferred between the env if you want to maintain any form of state, which is why you can’t hold a state through different Lambda functions or different Lambda function invocations.
#### State Machines
![[AWS Step Function-1.png]]

*Step Functions* as a service lets you create what are known as state machines. Think of a **State Machine as a workflow**.

It has a start point and it has an Endpoint, and in between, there are states. State you can think of as things which occur inside the State Machine.

> **For the exam, you only need to remember that at a high level, standard is the default and it has a one-year execution limit. Express, that’s designed for high volume event processing workloads, such as IOT, streaming data processing and transformation, mobile application backends or any of those type of workloads. These can run for up to 5 minutes**

So you would use *Standard* **for anything that’s long-running** and *Express* **for things that are highly transactional and need much more in terms of processing guarantees**.
![[AWS Step Function-2.png]]
[[Simple Queue Message (SQS)]]
