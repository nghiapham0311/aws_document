![[Kinesis Data Firehose.png]]
What *Kinesis*, by *default*, *doesn’t offer is a way to persist that data*. Once records in Kinesis age past the end of the rolling window, then they’re gone forever. Now *Kinesis Data Firehose* is a **fully managed service to deliver data to supported services, including S3, which lets data be persisted beyond the rolling window of Kinesis Data Streams**.

Data Firehose is also used to load data into data lake products, data stores, and analytics services within AWS. So Data Firehose scales automatically. It’s fully serverless and it’s resilient.

Firehose accepts data and it offers **near-realtime** delivery of that data to destinations.

> **The key for the exam. It is not a real time product. It is a near-real time product**

So generally the *delay* is *anywhere around the 60 second mark*. So it’s not like *Kinesis*, which **offers consumers fully real time access to data**, which is ingested. *Firehose* is **near-real time**.

Firehose also supports the transformation of data on the fly using Lambda. Anything that you can define in a Lambda function can be done to data being handled by Firehose, but be aware that it can add latency, depending on the complexity of the processing.

Firehose is a pay-as-you-go service. You are billed based on data volume passing through the service. So it’s a really cost-effective service which handles the delivery of data through to supported destinations.
![[Kinesis Data Firehose-1.png]]
The end result of Firehose is to deliver incoming data through to a number of supported destinations.

> **These are important for the exam. You need to be able to pick if Firehose is a valid solution. And for that, you need to know the valid destinations for the service. So it can deliver data to HTTP endpoints, which means it can deliver to 3rd providers**

It directly supports delivery to Splunk. It can deliver data into Redshift. It can also deliver data into ElasticSearch. And then finally, it can deliver data into S3

Firehose can directly accept data from producers, or that data can be obtained from a Kinesis Data Stream. So if you already have a set of producers adding data into a Kinesis Data Stream, then we might want to integrate that with Firehose.

> Remember, these producers are adding data into the Kinesis stream.

That data is available in real time by any consumers of that stream. But Kinesis offers no way to persist that data anywhere, or no way to deliver it natively to any other services.

But what we can do is to integrate the Kinesis data stream with the Kinesis Firehose delivery stream. That data is delivered into Firehose in real time. Now producers can also send data directly into Firehose. If you have no need for the features that Kinesis Data stream provide, or you just want to use Firehose directly.

In any case, Firehose actually receives the data in real time, but this is where that changes. So even though Firehose receives data in real time, Firehose itself is not a real time service. Kinesis data streams are real time, but Firehose is what’s known as a near-real time service. What this means in practice is that any data being handled by the service is buffered for delivery.

Firehose waits for 1MB of data or 60 seconds. These can be adjusted, but these are the general minimums of the product. So for low volume producers, Firehose will generally wait for the full 60 seconds and then deliver that data through to the destinations. For high volume producers, it will deliver every MB of data that’s injected into the product. So even though Firehose gets the data in real time, it doesn’t deliver it to the destination in real time.

**From an AWS perspective, something in the range of 200 milliseconds would be a real time product, but something in the range of 60 seconds would be classified as near-real time. You need to get a feel for the differences between those two, and which products fit into which of those categories.**

Firehose can actually transform the data passing through it, using Lambda. So source records added to Firehose are sent to a Lambda function and functions can be created from blueprints to perform common tasks, and then transformed records are sent back for delivery. But this can add to the latency of data flowing through the product.

If you decide to do a transform, then you can optionally store the unmodified data in a backup bucket, which you define. Once the buffer or time buffer passes, then data is passed into the final destinations. So transformed records can be sent into S3 or directly to ElasticSearch or directly to Splunk, or HTTP endpoints.

The only exception to this architecture for delivery is when you are using Redshift is it uses an intermediate S3 bucket and then runs a Redshift copy to bring the data from S3 into the product. So even though conceptually, it’s direct, when used, you’re actually copying data to an intermediate location, an S3 bucket, and then you’re running the copy command to pull that data into Redshift, and that’s handled all end to end by the Data Firehose product

There are a few common situations where Firehose will be used. You might use it to provide persistence to data coming into a Kinesis stream. So providing a permanent storage of data that comes into a stream. So it’s not lost when it exits the rolling window that Kinesis data streams provide, or you might use it if you want to store data in a different format, because Firehose can transform it using Lambda. Or you might want to deliver data that comes either directly into Firehose or via data stream into one of the supported products.

[[Kinesis Data Analytics]]