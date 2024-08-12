![[Lambda Function.png]]
*Lambda* is a *function as a service* or a fast product. This means that *you provide specialized, short-running and focused code* to Lambda, and *it takes care of running it and billing you only for what you consume*.

So a Lambda function is a piece of code which Lambda runs and every Lambda function is using a supported runtime.

An example of a supported runtime is Python 3.8. So when you create a Lambda function, you need to define which runtime that piece of code uses.

When you *provide your code to Lambda*, it’s *loaded* into and *executed* within a *runtime env* and this runtime env is *specifically created* to run code *using a certain runtime, a certain language*, so when you create a Lambda function that uses the Python 3.8 runtime, than the runtime env that’s created is itself specifically designed to run Python 3.8 code.

When you create a Lambda function, you *also define* the amount of *resource that a runtime env* is provided with, so you directly *allocate* a certain amount of *memory*, and based on that amount of memory, a certain amount of *virtual CPU is allocated*, but this is **indirect**.

You don’t get to choose the amount of CPU, this is based on the amount of memory.

> The **key** thing to understand about *Lambda as a service*, because *it’s a function as a service* product because it’s *designed* for *short-running and focused functions*, you’re only actually **billed for the duration that a function runs**. So based on the amount of resource allocated to an env and based on the duration that that function runs for per invocation, that determines how much your billed for the Lambda product, so you’ll billed for **the duration of function executions**.
#### Lambda Architect
Lambda is a key part of serverless architectures running within AWS.
![[Lambda Function-1.png]]
Architecturally, the way that Lambda works is this. You define a Lambda function. You can *think* of a *Lambda function* as *a unit of configuration*.

You can also use the term Lambda function to describe the actual code, but when you think of a *Lambda function*, think of it **as the code, plus all the associated wrappings and configuration**.

Lambda *supports* lots of *different runtimes*. Some of the common ones are various different versions of Python. We also have *Ruby*, we’ve got *Java*. We’ve also got *Go*, and there’s also *C#*, as well as various versions of *NodeJS*.

> For the **exam**, one really important point is that if you see or hear the term Docker, consider this to **mean not Lambda**. So **Docker is an anti-pattern for Lambda**. Lambda **does now support using Docker images** but this is **distinct from the word Docker**. If you hear the term Docker in the exam, then it generally will be referring to traditional containerized computing, so that’s using a specific Docker image to spin up a container and use it in a containerized compute env, such as **ECS**

Conceptually, think about it like this. **Every time** a *Lambda function* is invoked, which means to execute that function, a *new runtime env is created* with *all of the components that that Lambda function needs*, for example, a Python 3.8-based Lambda function. So the code loads, it’s *executed* and *then it terminates*. *Next time*, a **new**, clean env is created, it does the same thing and then it terminates.

Lambda functions are *stateless*, which means no data is left over from a previous invocation. Every time a function is invoked, it’s a brand new invocation, a branch new env.

You directly define the **memory**, and this is anywhere from **128 MB to 10240 MB** in one MB steps. **Don’t** directly **control the amount of virtual CPU**, this scales with the memory. The runtime env also has some **disc space allocation**. **512 MB** is mounted as /tmp within the runtime env. This is the default amount, but it can scale to **10,240 MB**. You have to assume that it’s blank every single time => only be viewed as temporary space.

Lambda functions **can run** for up to **900 seconds, or 15 minutes**, and this is known as the **function timeout**. This is important because for anything *beyond 15 minutes*, you can’t use Lambda directly and that’s a really important figure to know for the exam.

You can use other things such as *step functions* to **create longer-running workflows**, but one indication of one function has a maximum of 15 minutes, or 900 seconds.

The *security* for a *Lambda function* is controlled using *execution roles* and these are *IAM roles* assumed by the Lambda function, which provides permissions to interact with other AWS products and services, so any permissions which a Lambda function needs to be provided with are delivered by creating an execution role and attaching that to a specific Lambda function.
#### Common Uses
![[Lambda Function-2.png]]
So Lambda forms a core part of the delivery of serverless applications within AWS, and generally this uses products such as S3, API Gateway and Lambda, so these 3 together are often used to deliver serverless applications.

Lambda can also be used for file processing, using S3, S3 Events and Lambda, so a very common example that’s used in training is watermarking images, so have images uploaded to S3, generate an S3 Event, invoke a Lambda function, which applies a watermark, and then terminates and you’re only billed for the compute resources used during those Lambda function invocations.

You can also use Lambda for db triggers, so this is using DynamoDB as well as DynamoDB Streams and then Lambda, so Lambda can be invoked anytime data is inserted, modified or deleted from a DynamoDB table with streams enabled, and this is another powerful architecture.

You can also use Lambda to implement a form of serverless CRON so you can use EventBridge or CloudWatch Events to invoke Lambda functions at certain times of day or certain days a week to perform certain scripted activities and this is something that traditionally you would need to run on something like an EC2 instance, but using Lambda means that you’re only billed for the amount of time that these functions are executing, so this is another really common use case.

Finally, you can perform real time stream data processing. So Lambdas can be configured to invoke whenever data is added to a Kinesis Stream and this can be useful because Lambda is really scalable and so it can scale with the amount of data being streamed into a Kinesis Stream. This is another really common architecture for any businesses that are streaming large quantities of data into AWS and they require some form of real time processing.

[[Lambda Deepdive]]

