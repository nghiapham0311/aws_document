![[SQS Dead-Letter Queues.png]]
Dead-Letter Queues are designed to help you handle reoccurring failures while processing messages which are within an SQS queue.

When you’re using dead-letter queues in the real world, all SQS queues have retention periods for messages. So if a message ages past a certain point and hasn’t been processed, then that message is dropped.

A single dead-letter queue can be used for multiple source queues

[[Kinesis Data Streams]]