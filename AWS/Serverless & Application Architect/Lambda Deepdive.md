#### Public Lambda
![[Lambda Deepdive.png]]
*Lambda* has **2** networking modes and you need to be aware of both of them for the exam.

First we have *public*, which is the *default* and then second, we have *VPC networking*.

For public networking, we start with an AWS env and inside it, a single Lambda function. This is part of a wider application. Let’s say the Categram enterprise application running in a VPC, which uses Aurora for the db, EC2 for compute and the EFS for shared file storage.

This is the default configuration for Lambda where it’s running in the public AWS network. So Lambda, using this configuration can access public space AWS services, such as SQS and DynamoDB or internet based services, such as IMDb, if the Lambda function wanted to fetch the latest details of cat themed movies and TV shows.

So Lambda running by default using public networking means that it has network connectivity to public space AWS services and the public internet. It can connect to both of those from a networking perspective, and as long as it has the required methods of authentication and authorization, then it can access all of those services.

*Public networking* offers the **best performance for Lambda** because no customer specific networking is required. Lambda functions can run on shared hardware and networking with nothing specific to one particular customer. But this does mean that any Lambda functions running with this **default have no access to services running within a VPC**, unless those services are configured with public addressing as well as security rules to allow external access. **So this is a big limitation that you need to understand for the exam.**

With the architecture, this Lambda function, could not access Aurora, EC2 or the EFS unless they had public addressing and the security was configured to allow that access.

In this example, without configuration changes, the Lambda function could access public services but would have no access to anything running inside the VPC.
#### Private Lambda
![[Lambda Deepdive-1.png]]
This is the same subnet where the Categram enterprise infrastructure is running from.

> For the **exam** specifically, the key thing to understand about *Lambda’s running inside a VPC* is that **they obey all of the same rules as anything else running in a VPC** because they’re actually running within that VPC

The flip side of this means *they can’t access things outside of the VPC* unless networking configuration exists within the VPC to allow this external access. So by default, with this architecture, the Lambda function couldn’t access DynamoDB or any internet based endpoint, such as with this example, IMDb

> If you face any **exam** questions or you need to design any solutions which involve **Lambda functions running within a VPC**, then just **treat them like anything else running in that VPC**. This means that you could *use a VPC endpoint, for example a Gateway endpoint, to provide access to DynamoDB*.

Because the Lambda function is running within the VPC, it could utilize a gateway endpoint to access DynamoDB. Or in the case that the Lambda function needed access to AWS public services or the internet, you could deploy a NAT gateway in a public subnet then attach an internet gateway to the VPC.

The same gateways and configurations are needed to allow VPC based Lambda functions to communicate with the AWS public Zone and the public internet.

You also need to give your Lambda functions EC2 network permissions via the execution role, because the Lambda service needs to create network interfaces within your VPC, it requires these permissions.
#### Security
![[Lambda Deepdive-3.png]]
When it comes to *Lambda permissions* there are actually **2** key parts of the permissions model that you need to understand. One of them is pretty well known.

For this env running in Lambda to access any AWS products and services, it needs to be provided with an execution role. This is a *role which is assumed by Lambda* and by doing so, the *code within the env gains the permissions of that role based on the roles permissions policy*. So a role is created, which has a trust policy which trusts Lambda and the permissions policy that that role has, it used to generate the temporary credentials that the Lambda function uses to interact with other resources.

![[Lambda Deepdive-4.png]]
This in many ways is like a bucket policy on S3. It controls who can interact with a specific Lambda function. It’s this *resource policy*, which can be used to *allow external accounts to invoke a Lambda function or certain services to use a Lambda function*, such as SNS or S3.

The resource policy is something changed when you integrate other services with Lambda and you can manually change it via the CLI or the API. This is only something which can be manipulated using the CLI or the API.
#### Logging
![[Lambda Deepdive-5.png]]
> Remember the terms X-Ray and distributed tracing because that might come in handy for one or 2 exam questions.

**One really important thing to remember for the exam is that for Lambda to be able to log into CloudWatchLogs, to generate the output of any of the executions, you need to give Lambda permissions via the execution role.**

So there’s actually a pre-built policy and role within AWS specifically designed to give Lambda functions the basic permissions that they require to log information into CloudWatchLogs.

One really common exam scenario is where you’re trying to diagnose why a Lambda function is not working, there’s nothing in CloudWatchLogs. And one possible answer is that it doesn’t have required permissions via the execution role.
#### Invocation
![[Lambda Deepdive-6.png]]
We’ve got 3 different methods for invoking a Lambda function.

We’ve got Synchronous invocation, Asynchronous invocation and invocation using Event Source mappings
##### Synchronous Invocation
![[Lambda Deepdive-7.png]]
With this model, you might *start* off with a *command line or API directly invoking a Lambda function*. The Lambda function is provided with some data and it executes that data.

All this time, the *command line or API* is **waiting for a response**, because its **synchronous**, it needs to wait here until the Lambda function completes its execution. So the *Lambda function* finishes and **it returns that data whether it’s a success or a failure**.

Synchronous invocation also happens if Lambda is used indirectly via the API Gateway, which is the use case for many Serverless architectures, so we might have some clients using a web application via API Gateway, and this proxies through to one or more Lambda functions. Again, the Lambda function performs some processing all the while the client is waiting for a response within their web application.

When the Lambda function responds, this goes back via the API Gateway and back through to the client. The common factors with both of these approaches is that the client sends a request which invokes Lambda, and the result, be it a success or failure, is returned during that initial request, the client is waiting for any data to be returned

Another implication of a Synchronous invocation is that any errors or retries have to be handled within the client. The Lambda function runs once, it returns something and then it stops.

If there’s a problem or data isn’t processed correctly then the client needs to rerun that request, and this happens at the client side.

**So Synchronous invocation is generally used when it’s a human directly or indirectly invoking a Lambda function**
##### Asynchronous
![[Lambda Deepdive-8.png]]
This is *typically* used when *AWS Services* **invoke Lambda functions** on your behalf.

An S3 bucket with S3 events enabled, so we upload a new images of whiskers to this S3 bucket. This causes an event to be generated and sent through to Lambda, and this is an Asynchronous invocation.

So S3, isn’t waiting around for any kind of response. It basically, just forgets about it at this point. Once it’s sent that event through to Lambda, it doesn’t continue waiting. It doesn’t worry about this event at all.

Maybe as part of processing this image it’s generating a thumbnail or maybe performing some kind of analysis and storing that data into DynamoDB. But again, S3 isn’t waiting around for any of this it’s asynchronous.

Lambda is responsible for any reprocessing in the event that there’s a failure. This reprocessing value is configurable between 0 and 2 times. A key requirement for this is that the function code needs to be idempotent.

> You can run it as many times as you want and the out come will be the same ⇒ idempotent

When Lambda retries an operation, it doesn’t really provide any other information. The function just reruns. So logically in this example, you would need to make sure that your function code isn’t additive or subtractive. It just needs to perform its intended task.

Generally, when designing a Lambda function, which is used in this way, the Lambda function needs to finish with a desired state. It needs to make something true. If you’re using Lambda functions, which are designed in a non-idempotent way, you can end up with some questionable results.

Lambda can be configured to send any events, which it can’t process after those automatic retries to a `dead letter queue`, which can be used for diagnostic processing.

A new feature of Lambda is the ability to create destinations. So events processed by Lambda functions can be delivered to another destination such as SQS, SNS, another Lambda function, and even EventBridge. Separate destinations can be configured based on successful processing or failures.

**It’s generally used by AWS Services, which are capable of generating events and sending those events to Lambda. It means that Lambda can automatically reprocess failed events and the original source of the event isn’t waiting for processing to complete**
##### Event Source Mapping
![[Lambda Deepdive-9.png]]
This is typically used on *Streams or queues*, which **don’t generate events**. So things **where some kind of polling is required**.

Let’s say that we have a Kinesis data Stream and into this Stream a fleet of producer vans driving around scanning with LIDAR and imaging equipment are all producing data which is being put into a Kinesis Stream.

Kinesis is a Stream-based product. Generally, consumers can read from a Stream, but it doesn’t generate events when data is added. Historically, this wouldn’t have been an ideal fit for Lambda, which is an Event-Driven Service.

So what happens is that we have a hidden component called an *Event Source mapping*, which is *polling queues or Streams looking for new data and getting back source batches*. So batches of source data from this data source.

Now these source batches are then broken up as required based on a batch size and sent into a Lambda function as event batches. A single Lambda function invocation could in theory receive hundreds of events in a batch. It depends on how long each event takes to process.

> Lambda has a 15 minute timeout so you need to carefully control this event batch size to ensure that Lambda function doesn’t terminate before completing this batch

**There’s one really important thing that you need to understand about Event Source mapping. With Asynchronous invocations an event is delivered to Lambda from the source and Lambda doesn’t need permissions to the source Service unless it actually wants to read more data from that source.**

For example, if an object is added to an S3 bucket then S3 generates and delivers an event, which contains details of that event. So which object was uploaded and perhaps some other Metadata, but unless you need to read additional data from S3, maybe to get the actual object, then the Lambda function doesn’t need S3 permissions.

**With Event Source mapping invocation, the source Service isn’t delivering an event. The Event Source mapping is reading from that source. So the Event Source mapping uses permissions for the Lambda Execution Role to access the source Service. This is really important to know**

**So even if a Lambda function receives an event batch containing Kinesis data, even though the Lambda function doesn’t directly read from Kinesis, the Execution Role needs Kinesis permissions, because the Event Source mapping uses them on its behalf to retrieve that data**

Any batches which consistently fail can be sent to an SQS queue or an SNS topic for further processing or analysis.
#### Lambda versions
![[Lambda Deepdive-10.png]]
With Lambda functions it’s possible to define specific versions of Lambda functions. So you could have different versions of a given function.

As it relates to Lambda, a version of a function is actually the code plus the configuration of that Lambda function. So the resources and any env variables in addition to any other configuration information.

When you publish a version, that version is immutable. It never changes once it’s published and it even has its own Amazon resource name. So once you publish a version you can no longer change that version.

There’s also the concept of `$Latest` and `$Latest` points at the latest version of a Lambda function. This can obviously change as you publish later and later versions of the function. So this is not immutable.

You can also create aliases. So for example, DEV, STAGE and PROD, and these can point at a particular version of a Lambda function and these can be changed. So these aliases are not immutable.

So generally, with large scale deployments of Lambda you’d be producing Lambda function versions for all of the major changes and using aliases so that different components of your serverless application can point at those specific immutable version numbers.
#### Lambda startup times
![[Lambda Deepdive-11.png]]
> You need to be careful, because your functions need to be able to cope with the env being new and clean every time. They can never assume the presence of anything

So when you create a Lambda function generally, most things go within the Lambda function handler. But if you create *anything outside of the Lambda function* handler then *these will be made available for any future function invocations in the same context*. So anything you define within a Lambda function handler is limited to that one specific invocation at that Lambda function. But for anything which you **anticipate there being a potential for reuse**, you can **declare that outside of the Lambda function** handler, and in theory, that will be available for any other invocations of Lambda function, which occur within that same execution context.

[[CloudWatchEvents & EventBridge]]
