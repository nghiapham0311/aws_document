![[SQS Delay Queues.png]]
*Delay queues*, at a high level, allow you to *postpone* the *delivery of messages* to consumers. The concept is simple enough.

So we start with an SQS queue. And inside this queue, we send a single message which is added to the queue using the SendMessage operation. Once a message is in the queue, messages can be polled using ReceiveMessage. While the message is being processed, the Visibility Timeout takes effect. During this time, any further ReceiveMessage calls will return no results.

During this processing period, either the process will complete and the message will be explicitly deleted or not. If not, this suggests a failure in processing and the message will reappear on the queue.

The *visibility timeout* period is *configurable*. The *default* is *30 seconds* and the valid *range is 0 seconds through to 12 hours*. This value can be changed on a per queue or per message basis. In which case, it’s changed with the ChangeMessageVisibility operation. The critical thing to understand about visibility timeout, is that messages need to appear on the queue and be received before this visibility timeout occurs. So this is used to allow automatic reprocessing.

So you receive messages from a queue and you begin processing. If that processing fails and the application doing it crashes, it might not be in a position where it can tell the queue that processing has failed. So visibility timeout means that after a certain configuration duration, that message will reappear in the queue and can be processed again.

**Visibility timeout is generally used for error correction and automatic reprocessing.**

A delay queue is significantly different. With a delay queue, we configure a value called DelaySeconds on that queue. This means that messages which are added to the queue will start off in an invisible state for that period of time. So when messages are added, they’re conceptually park ed or invisible for that duration of time. They’re not available on the queue. During this DelaySeconds period, any ReceiveMessages operation will return nothing.

Once the period expires, the message will be visible on the queue. Now, the default is 0 and for a queue to be a delay queue, it needs to be set to a non-zero value. And the maximum is 15 minutes. You can also use message timers to configure this on a per message basis. This has the same minimum of 0 and maximum of 15 minutes

> **You can’t use this per-message setting on FIFO queues. It’s not supported.**

Delay queues, are similar to visibility timeouts because both features make messages unavailable to consumers for a specific period of time.

But the difference between the 2, is that for *delay queues*, a message **is hidden automatically when it’s first added to the queue**. Using visibility timeouts, a message is initially visible and it’s only hidden after it’s consumed from the queue and automatically reappears if that message isn’t deleted.

So *delay queues* are generally **used when you need to build in a delay in processing into your application**. Maybe you need to *perform a certain set of tasks before you begin processing a message*. Or maybe you want to add a certain amount of time between an action that a customer takes and for the processing of the message that represents that action.

*Visibility timeouts* are *used to support automatic reprocessing of problematic messages*.

[[SQS Dead-Letter Queues]]