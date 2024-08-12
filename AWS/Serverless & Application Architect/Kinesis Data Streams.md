*Kinesis data streams* are a *streaming service within AWS* **designed to ingest large quantities of data and allow access to that data for consumers**.

Kinesis is ideal for dashboards and large scale real time analytics needs.

Kinesis data firehose allows the long term persistent storage of kinesis data onto services like S3
#### Kinesis Concepts
![[Kinesis Data Streams.png]]
It’s designed to ingest data, lots of data from lots of devices, or applications. Producers send data into a Kinesis stream. The stream is the basic entity of Kinesis, and it can scale from low levels of data throughput to near infinite amounts of data.
#### Kinesis Architecture
![[Kinesis Data Streams-1.png]]
The stream ingests this data and it’s from this that consumers read the data. The way that Kinesis Stream scales is by using a shard architecture. A stream starts off with one shard, and as additional scale is required, shards are added to the stream. Each shard provides its own capacity, one MB per second of ingestion capacity, and 2 MB per second of consumption. **The more shards a stream has, the more expensive it is, and the more performance that it provides**.

What also impacts the price is the data window. By default, a stream provides a 24 hour window, and this can be increased up to 365 days for additional cost. The window is also persistent, so a 365 day window means 365 days’ worth of data stored by Kinesis.

The way that the data is stored on a stream is via Kinesis Data Records, and these have a maximum size of one MB. Kinesis Data Records are stored across shards, meaning the performance scales in a linear way based on the number of shards.

Kinesis also has a related product called the Kinesis Data Firehose. This connects to a Kinesis Stream, and can move the data which arrives onto a stream on mass into another AWS service, like S3.

So if you have a fleet of sensors which stream data into Kinesis, and that’s used for real-time analysis, but if you also need to store this longer term, maybe to analyze it using a different AWS product, such as EMR, which is a big data analytics tool, then you can put that data into S3 using Kinesis Firehose.
#### SQS vs Kinesis
![[Kinesis Data Streams-2.png]]
> **Is the question about the ingestion of data, or is it about worker pools, decoupling, or does it mention asynchronous communication? If it’s about the ingestion of data, it’s going to be Kinesis. If it’s about any of the others, then assume it’s SQS first, and only change your mind if you have strong reasons to do so**

[[Kinesis Data Firehose]]