![[SQS Standard vs FIFO Queues.png]]
Think about *FIFO queues* as **single lane highways**, and then think about *standard queues* as **multi-lane highways**.

Imagine the messages as cars driving along these highways. What this means is that the performance of a FIFO queue, so the number of messages per second in reality is limited by the width of the road.

*FIFO queues* can handle **300 messages per second** without batching and **3000 with**. This is actually 300 transactions per second to the SQS API when using FIFO mode. Each transaction is one message. But with batching, it means that each transaction can contain 10 messages. It’s worth mentioning at this point that there is a high throughput mode for FIFO.

Standard queues, so multi-line highways, don’t suffer from any real performance issues and can scale to near infinite number of transactions per second.

FIFO queues, guarantees order. They’re first in, first out. So what you’re trading is performance for this preserved order. They also guarantee exactly-once processing, removing the chance of duplicate message delivery.

**Another odd restriction is that FIFO queues have to have a FIFO suffix in order to be a valid FIFO queue.**

FIFO queues are great for workflow-based order processing, command ordering, so if you’ve got a system administrator who’s entering commands into a processing system and you need the order of those commands to be maintained, then FIFO queues are ideal, as well as any sequential iterative price adjustment calculations for sales order workflows.

Standard queues, so the multi lane highways of queues, they’re faster. Conceptually think of this as multiple messages being carried on the highway at the same time. Because of this, there are a few important trade offs.

First, there’s no rigid preservation of message ordering. It’s best efforts only.

Second, what’s guaranteed is only at-least-once message delivery. Meaning in theory, messages can be delivered more than once. So any application which use standard queues need to be able to accommodate the potential for multiple of the same messages to be delivered.

**Standard queues are ideal for decoupling application components, or for workable architectures, or to batch together items for future processing. So all of these are ideal use cases for standard SQS queues.**
#### SQS Extended Client Library
![[SQS Standard vs FIFO Queues-1.png]]

> The exam often mentions *Java* and using the *Extended Client Library*, so if you see any *requirements* to *send messages* to an *SQS* larger than **256KB**, then just keep in mind that you can use the **Extended Client Library**, and equivalent libraries which perform the same task are available for all of the different supported languages when using those with AWS.

[[SQS Delay Queues]]