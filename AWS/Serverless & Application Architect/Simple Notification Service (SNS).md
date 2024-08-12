The Simple Notification Service or SNS .. **is a PUB SUB style notification system** which is used within AWS products and services but can also form an essential part of serverless, event-driven and traditional application architectures.

Publishers send messages to TOPICS

Subscribers receive messages SENT to TOPICS.

SNS supports a wide variety of subscriber types including other AWS services such as LAMBDA and SQS
![[Simple Notification Service (SNS).png]]
*SNS* is a *highly available, durable, secure, pub-sub messaging service*. It’s a **public AWS service** meaning to access it, you need network connectivity with the public AWS endpoints. But the benefit of this is that it’s accessible from anywhere that has that network connectivity.

What it does at a high level, is coordinate the sending and delivery of messages. And messages are payloads which are up to **`256kb`** in size.

Architecturally, messages are not designed for large binary files. The *base entity of SNS* is the **SNS topic**. It’s *on these topics* where *permissions are controlled* as well as where most of *the configuration for SNS is defined*. SNS has the concept of a publisher, and a publisher is the architectural name for something which sends messages to a topic.

With pub-sub architecture, publishers send things into a topic. As well as publishers, each topic can have subscribers. These by default receive all of the messages which are sent to the topic.

Subscribers can come in many different forms, we’ve got things like HTTP and HTTPS endpoints, email addresses which can receive the message, SQS queues where each message is added to the queue as it’s sent to the topic, topics can also be configured with mobile push notification systems as subscribers, so that messages sent to are topic delivered to mobile phone as push notifications, or an SMS messages, even Lambda function can be subscribed to a topic.

So that that Lambda function is invoked, as messages are sent into the topic. SNS is used across AWS products and services. CloudWatch uses it when alarms change state, CloudFormation uses it when stacks change state. ASG can be configured to send notifications to a topic when a scaling event occurs.
#### SNS Architect
![[Simple Notification Service (SNS)-1.png]]
the *SNS* service, is it *public space*, AWS service. It operates from the AWS public zone. Because of that, the service *can be accessed from the public internet*, assuming the entity trying to access it has the relevant AWS permissions and VPC is configured to be able to access public AWS endpoints, then SNS can be accessed from a VPC as well.

SNS as a service runs from this public zone and you can create topics inside of SNS. For each topic, a wide variety of producers so external APIs ruling on the public internet or CloudWatch or EC2 or ASG or CloudFormation stacks and many other AWS services can publish messages into a topic.

Topic has subscribers and things can be subscribers and producers at the same time. For example, APIs. Any subscribers by default, will receive all of the messages sent to the topic by publishers.

But it’s possible to apply a filter onto a subscriber which means that subscriber will only receive messages which are relevant to its particular functionality.

Another interesting architecture is the **Fanout architecture**, and this is when you have a **single SNS topic with multiple SQS queues as subscribers**. *This is the way that you can create multiple related workloads*. So if for example, a message is sent to an SNS topic when a processing job arrives, that messages can be added to multiple SQS queues which are configured as subscribers for that SNS topic.

Each of these cues might perform the same related processing, but using that pet tube as an example, might work on different variant. so they might be processing a different bit rate or a different video size. So using **fan out**, it’s a great way of *sending a single message* to an *SNS topic* **representing a single processing workload and then fan that out to multiple SQS queues, to process that workload in slightly different and isolated ways**

The functionality that’s offered by SNS is pretty important. It really is a foundational service for developing application architectures within AWS platform.

![[Simple Notification Service (SNS)-2.png]]
SNS offers delivery status. So with a number of different types of subscribers you can confirm the status of delivery of messages to those subscribers. Examples of some subscriber types that do support delivery status is HTTP or HTTPS endpoints, Lambda and SQS.

As well as delivery status, SNS also supports delivery retries, so you’ve got the concept of reliable delivery within SNS.

*SNS* is also a *HA* and *scalable service* within a **region**. So *SNS* is a **regionally resilient service**. So all of the data that’s sent to SNS is replicated inside a region, it’s scalable inside that region so it can cope with a range of workloads from nothing all the way to highly transactional workloads and it’s also highly available.

If particular AZ fail, then an SNS topic will continue to function. SNS also capable of service-side encryption or SSE, which means that any data that needs to be stored persistently on desk can be done so, in an encrypted form.

> If you do have any requirements which mandate the use of on disk encryption

SNS topics are also capable of being used cross accounts, just like S3 buckets you can apply a resource policy in case of an SNS topic, this is a topic policy, it’s exactly the same, it’s a resource policy that you apply to the resource, the SNS topic and you can configure from a resource perspective exactly what identities have access to that topic.
[[AWS Step Function]]