![[Simple Queue Message (SQS).png]]
*SQS* provides *managed message queues*. It’s a *public service*, so it’s accessible anywhere with access to the AWS public space endpoints. This includes private VPCs if they have connectivity to the services.

It’s fully managed, so it’s delivered as a service, you create a queue and the service delivers that queue as a service.

Queues are highly available and highly performant by design. So you don’t need to worry about replication and resiliency. It happens within a region by default.

*Queues* come in one of **2** types: *Standard* queues and *FIFO* queues.

FIFO queue guarantee an order. So if messages, one, two and three are added in that order to a FIFO queue then when you receive messages you’ll also see them in order, so one, two and three.

With a Standard queue, this is best efforts, but there’s always the possibility with a Standard queue that messages could be received out of order.

The message which are added to a queue can be up to `256KB` in size. **If you need to deal with any data which is larger, then you can store it on something like S3 and link to that object inside the message.**

Architecturally though ideally, you want to keep messages small, because they’re easier to process and manage at scale.

The way that a queue works is that clients can send messages to that queue and other clients can poll the queue. *Polling* is *the process of checking for any messages on a queue*. When a *client polls and receives messages*, those **messages aren’t actually deleted from the queue**. They’re actually **hidden** for a period of time, **the VisibilityTimeout**.

The VisibilityTimeout is the amount of time that a client can take to process a message in some way. So if the client receives messages from the queue if it finishes processing whatever workload that, that message represents then it can explicitly delete that message from the queue, and that means that it’s gone forever. But if a client doesn’t explicitly delete that message then after the VisibilityTimeout the message will reappear in the queue.

**Architecturally, this is a great way of ensuring fault tolerance because it means that if a client fails when it’s processing a job or maybe even fails completely, then the queue handles the default action to put the message back in the queue, which makes that message available for processing by a different client. So VisibilityTimeout is really important**

> VisibilityTimeout is the amount of time that a message is hidden when it’s received. If it’s not explicitly deleted then it appears back in the queue to be processed again

SQS also has the concept of a Dead-Letter Queue, this is a queue where problem messages can be moved to, for example, if a message is received five or more times and never successfully deleted, then one possible outcome of that can be to move the message to the Dead-Letter queue. Dead-Letter queues allow you to do different sets of processing on messages that can be problematic.

So if messages are being added to the queue in a corrupt way or if there’s something specific about these messages that means different styles of processing is required then you can have different workloads looking at the Dead-Letter queue.

Queues are also great for scaling, *ASG* **can scale based on the length of the queue** and *Lambdas* **can be invoked when messages appear on a queue**, this allows you to build complex Worker Pool style architectures.
![[Simple Queue Message (SQS)-1.png]]
You might have 2 ASGs, one on the right is the web application pool and one on the left is Worker Pool.

A customer might upload a master video to the web application pool via a web app. The master video is taken by this web application pool and it’s stored in a master video bucket and a message is also added to an SQS queue.

The message itself has a link to the master video. So the S3 location that the master video is located at and this avoids having to deal with unwieldy message sizes.

The Web Pool is controlled by ASG and its scaling is based on CPU load of the instances inside that ASG. Meaning that it grows out as the load on the system increases. The scaling of the Worker Pool is based on the length of the SQS queue, so the number of messages in the queue. As the number of messages on the queue increases the ASGs scales out based on this number of messages. So it adds additional EC2 instances to cope with the additional processing. So instances inside this ASG they all poll the queue and receive messages. These messages are linked to the master video, which they also retrieve.

They perform some processing on that video. In this example, generating different sizes of videos and they store them in a different bucket. Then the original message that was on the queue is deleted.

If the processing fails or even if an instance fails, then it will be reprovisioned automatically by the ASG. The message that it was working on will automatically reappear on the queue after the VisibilityTimeout has expired.

As the queue empties, the number of worker instances scales back in all the way to 0 if no processing workloads exist. So the ASG that’s running the Worker Pool is constantly looking for the length of the queue.

When messages appear on the queue, the ASG for the Worker Pool scales out, adds additional instances, those instances poll the queue, retrieve the messages, download the master video from the master bucket, perform the transcode operations, store that in the transcode bucket, delete the message from the queue and then the size of Worker Pool ASG will scale back in as that workload decreases.
#### SNS and SQS Fanout
![[Simple Queue Message (SQS)-2.png]]
**You’ll take one single SNS topic with multiple subscribers, generally multiple SQS queues, and then that message will be added into each of those queues, allowing for multiple jobs to be started per object upload.**

![[Simple Queue Message (SQS)-3.png]]
Think of Standard queues like a multi-land highway and think of FIFO queues like a single-land road with no opportunity to overtake.

Standard queues guarantee `at-least-once` delivery, they make no guarantees on the order of that delivery.

FIFO queues both guarantee the order and guarantee `exactly-once` delivery, and that’s a critical difference.

Because FIFO queues are single lane roads, their performance is limited. 3000 messages per second with batching and 300 per second without.

So Standard queue scale in a much more linear and fluid way.

[[SQS Standard vs FIFO Queues]]
