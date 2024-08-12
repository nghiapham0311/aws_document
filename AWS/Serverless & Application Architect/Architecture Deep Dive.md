#### Monolithic
![[Architecture Deep Dive.png]]
This architecture has a number of consideration, a number of important things to keep in mind. Because it’s all *one entity*, it *fails together as an entity*. If *one component fails*, it *impacts the whole thing end to end*.

If uploading fails, it could also affect processing, as well as store and manage.

The other thing to consider when talking about *monoliths*, is they *also scale together, they’re highly coupled*. All of the *components* generally *expect to be on the same sever directly connected, and have the same codebase*.

Finally, this is one of the most **important** aspects of monolithic architectures that you need to be aware of, they **generally bill together**. All of the component of monolithic architecture are always running and because of that, they always incur charges, even if the processing engine is doing nothing, even if no videos are being uploaded, the system capacity has to be enough to run all of them. And so they always have allocated resources, even if they aren’t consuming them.
#### Tier Architect
![[Architecture Deep Dive-1.png]]
With a *tiered architecture*, the monolith is *broken apart*, what we have now is a *collection of different tiers*. And each of these *tiers* can be *on the same* server or *different servers*.

Components are still *completely coupled together* because each of the tiers connects to a single endpoint of another tier.

The upload tier needs to be able to send data directly at the processing tier.

But this architecture can be evolved even more. Instead of each tier directly connecting to each other tier, we can *utilize load balancers* located between each of the tiers.

This means it’s abstracted. It allows for **horizontal scaling**, meaning additional processing tier instances can be added. Communication occurs via the LBs, so the upload and store and manage tiers have no exposure to the architecture of the processing tier, whether it’s one instance or 100.

Downside:
- First, the *tiers are still coupled*. The upload tier, for example, expects and requires the processing tier to exist and to respond. If all processing tier after using LBs fail then the upload tier will failed because it expects at least one instance of the processing tier to answer it.
- The other issue with this architecture is that even if there’s no jobs to be processed, the processing tier has to have something running in case of request come from upload tier. So *it’s not possible to scale the individual tiers of the application back down to zero*.
#### Evolving with Queues
![[Architecture Deep Dive-2.png]]
*A queue* is a *system* which *accepts messages*, messages are sent onto a queue and messages can be received or polled off the queue. In many queues there’s ordering.

So in most cases, messages are received off the queue in a **FIFO**. Although it’s worth noting that this **isn’t always the case**.

Using a *queue based* **decoupled architecture**. Once the upload is complete instead of passing this directly onto the processing tier he does something slightly different. He stores the Master 4K Video inside an S3 bucket. It also adds a message to the queue detailing where the video is located as well as any of the relevant information such as what sizes are required.

This message because it’s the first message in the queue is architecturally at the front of the queue. At this point, the upload tier because it uploaded the master video to S3 and added a message to the queue, it’s finished this particular transaction. It doesn’t talk directly to the processing tier and it doesn’t know or care if it’s actually functioning.

The **key thing** is that the *upload tier* **doesn’t expect an immediate answer from the processing tier**. The queue has *decoupled* the upload and processing components. It’s moved from a synchronous style of communication where the upload tier expects and needs an immediate answer and it needs to wait for that answer.

Instead, he use *asynchronous or async communications*, where the upload tier sends the message, and it can either wait in the background or just continue doing other things while the processing tier does its job.

The other side of the queue we have *ASG*, which has been configured. It has a minimum size of 0, a desired size of 0 and a maximum size of 1337. So currently it has no instances provisioned but it has ASG policies which provision or terminate instances based on what’s called the queue length

The queue length is just the number of items in the queue. Because there are messages on the queue added by the upload tier, the ASG detects this. So the desired capacity is increased from 0 to 2. Because of this instances are provisioned by ASG. These instances start polling the queue and receive messages that are the front of the queue.

> Remember that these messages contain the data for the job but they also contain the location of the S3 bucket and the location of the object in that bucket

So once these jobs are received from the queue by these processing instances, they can also retrieve the master video from the S3 bucket. These jobs are processed by the instances and then they’re deleted from the queue. This leaves only one job in the queue. At this point, maybe the ASG decides to scale back because of the shorter queue length. So it reduces the desired capacity from 2 to 1. This process terminates one of the processing instances.

The instance that remains polls the queue and receives the one final message. It completes processing of that message, short performs the trans-coding on the videos, and it leaves 0 messages in the queue.

The *ASG* realizes this, it *scales back* to the desired capacity *from 1 to 0* and that results in the termination of the last processing easy to instance.

Using the *queue architecture*, so placing a queue in between 2 application tiers *decouples those tiers*. One tier adds jobs to the queue and doesn’t care about the health or the state of the other. And another tier can read jobs from that queue and it doesn’t care how they got there.

In this case, the processing tier which uses a worker fleet architecture it can scale anywhere from 0 to a near infinite number of instances based only on the length of the queue.

This is a really powerful architecture because of the asynchronous communications that it uses.
#### Microservice Architecture
![[Architecture Deep Dive-3.png]]
Microservices do individual things very well.

The upload service is a producer, the processing node is a consumer and the data store and manage microservice performs both.

Logically producers produce data or they produce messages. Consumers as the name suggests consume data or messages. Then you’ve got microservices that can do both things.

The things that services produce and consume architecturally are events. Queues can be used to communicate events but larger microservices architectures can get complex pretty quickly.

With services needing to exchange data between partner microservices, if we do this with a queue architecture then logically we’re gonna have a lot of queues. It works, but it can be complicated.

> Keep in mind a microservice is just a tiny self-sufficient application. It has its own logic, its own store of data and its own input-output components
#### Event Driven Architecture
![[Architecture Deep Dive-4.png]]
*Event-driven architectures* are just **a collection of event producers, which might be components of your application, which directly interact with customers** or they **might be parts of your infrastructure**, so it just easy to. Or they might be systems monitoring components.

*Components* or *services* within an application *can be both producers and consumers*. Sometimes a component might generate an event for example a failed upload and then consume events to force a retry of that upload.

The **key** thing to understand about *event-driven architecture* is that neither the *producers* or the *consumers* are *sat around waiting for things to occur*. They’re **not constantly consuming resources**, they’re not running at a 100% CPU load waiting for things to happen.

With *producers*, events are *generated* when something happens, when a button is clicked, these producers produce events.

*Consumers* are not waiting around for those events, they *have those events delivered*. And when they receive an event they *take an action* and then they *stop*. They’re **not constantly consuming resources**.

Now applications would be really *complex* if every software component or service *needed to be aware of every other component*. If every application component required a queue between it and every other component to put events into an access them from, it would be a really complex application architecture.

Best practice event-driven architectures have what’s **called an event router**, a highly available central exchange point for events. And the event router has what’s known as an **event bus**. You can think of this like a constant flow of information.

![[Architecture Deep Dive-5.png]]
Event-driven architectures only consume resources as and when required. So with an event-driven architecture, there’s generally nothing constantly running, nothing waiting for things. We’re not constantly polling hoping for things to happen. We have producers which generate events when something happens.

So if you’re browsing the [amazon.com](http://amazon.com) website and you click on order, that generates an event and actions are taken based on that event. But the [amazon.com](http://amazon.com) website is not constantly checking your browser each and every second to check if you’ve clicked submit on that order.

So producers generate events when something happens. So when clicks happen, when errors occur, when criteria are met, when uploads complete or any other actions.

So producers they generate event on things occurring and these events are delivered to consumers of those events. That generally happens using an event router.

An event router decides which consumers to deliver events to and when that occurs, when these events has delivered to the consumers, then actions are taken. Then once the action is complete, the system returns to waiting.

It goes into a dormant state and doesn’t consume resources.

So in summary, a mature event-driven architecture it only consumes resources while handling events. When events are not occurring, it doesn’t consume resources. This is one of the key components of a serverless architecture.

[[Lambda Fundamentals]]