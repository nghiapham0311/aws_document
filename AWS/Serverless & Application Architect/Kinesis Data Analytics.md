![[Kinesis Data Analytics.png]]
*Kinesis Data Analytics* is a service which **provides real-time processing of data which flows through it using the SQL**. Data inputs at one side, queries run against that dat in real time, and then data is output to destinations at the other.

The product *ingests* from *either Kinesis Data Streams or Kinesis Firehose* and can optionally pull in static reference data from S3. *After* data is processed, it *can be sent on in real time to destinations*. Currently, the *supported* *destinations are Firehose, and indirectly any of the destinations which Firehose supports.*

> If you are using *Firehose*, then *data* becomes *near real time* rather than real time.

The product also directly supports AWS Lambda as a destination, as well as Kinesis Data Streams. In both of those cases, the data delivery is real time. So you only have near real time if you choose Firehose or any of those indirect destinations if you use Lambda or Kinesis Data Streams then you keep the real time nature of the data.
![[Kinesis Data Analytics-1.png]]
**All of these are external sources or destinations. They exist outside of Kinesis Data Analytics.**
#### When and Where
![[Kinesis Data Analytics-2.png]]
[[AWS Cognito]]