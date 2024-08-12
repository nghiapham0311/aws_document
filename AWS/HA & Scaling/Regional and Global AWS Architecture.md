![[Regional and Global AWS Architecture.png]]
So at a global level, we have global service location and discovery. When you type [netflix.com](http://netflix.com) into your browser, what happen? How does your machine discover where to point at?

Next we’ve got content delivery, so how does the content or data for an application get to users globally? Are there pockets of storage distributed globally or is it pulled from a central location?

Lastly, we’ve got global health checks and failover. So detecting if infrastructure in one location is healthy or not and moving customers to another country as required.

Next we have regional components starting with the regional entry point and then we have regional Scaling and regional resilience and then the various application services and components
![[Regional and Global AWS Architecture-1.png]]
The Netflix client will use **DNS** for the *initial service discovery*. Netflix will have configured the DNS to point at one or more service endpoints.

Assume that there is a *primary location* for Netflix in a U.S region of AWS, maybe `us-east-1` and this will be used as the primary location. *If this fails*, then Australia will be used as a *secondary*.

Another valid configuration will be to send customers to their nearest location. In this case, sending our TV fans to Australia.

Assume we have a *primary* and a *secondary region*. So this is the DNS component to this architecture and **Route 53** is the implementation within AWS. Because of its flexibility, it can be configured to work in any number of ways. The key things for this global architecture though is that **it has health checks so it can determine if the US region is healthy** and direct all sessions to the US while this is the case or **direct sessions to Australia if there are problems with the primary region**.

Regardless of where infrastructure is located, a **content delivery network** *can be used at the global level*. This **ensures that content is cached locally as close to customers as possible** and these **cache locations are located globally and they all pull content from the origin location as required**.

The function of the architecture at this level is to get customers through to a suitable infrastructure location making sure any regional failures are isolated and sessions moved to alternative regions. It attempts to direct customers at local region at least if the business has multiple location.

And lastly, it attempts to *improve caching* using a *CDN* such as **CloudFront**
![[Regional and Global AWS Architecture-2.png]]
Initially, *communications from your customers* will generally *enter at the web tier*. Generally, *this will be a regional based AWS service* such as **an application load Balancer or API gateway** depending on the architecture that the application uses. The *purpose* of the *web tier* is to *act as an entry point for your regional based applications or application components*

It *abstracts* your customers away from the underlying infrastructure. It means that *the infrastructure behind it can scale or fail or change* **without impacting customers.**

The functionality provided to the customer via *web tier* is provided by the compute tier using services such as **EC2, Lambda or containers which use the ECS.**

So in this example, the **Load Balancer** will use **EC2** to provide compute services through to our customers. The *compute tier* though will *consume storage services, another part of all AWS architectures*.

This tier will use services such as **EBS (Elastic Block Store), EFS (Elastic File System) and even S3** for things like media storage.

You’ll also find that many global architectures utilize **CloudFront**, the global CDN within AWS and CloudFront is capable of using S3 as an origin for media. So Netflix might store movies and TV shows on S3 and these will be **cached by CloudFront.**

All of these tiers are separate components of an application and can consume services from each other. So CloudFront can directly access S3 in this case to fetch content for delivery to a global audience.

In addition to *file storage*, most envs require data storage, and within AWS, this is delivered using products like **RDS, Aurora, DynamoDB and Redshift for data warehousing.**

In order to improve performance, most applications don’t directly access the db, instead they go via a *caching layer*. So products like **ElasticCache for general caching or DynamoDB Accelerator known as DAX when using DynamoDB**. This way reads to the db can be minimized.

Applications will instead consult the cache first and only if the data isn’t present in the cache will the db be consulted and the contents of the cache updated. Now caching is generally in memory so it’s cheap and fast. Db tend to be expensive based on the volume of data required vs cache and normal data storage. So where possible, you need to offload reads from the db into the caching layer to improve performance and reduce costs.

Lastly, AWS have a suite of products designed specifically to provide application services. So things like **Kinesis, Step Functions, SQS and SNS**, all of which **provide some type of functionality to applications either simple functionality like email or notifications or functionality which can change an application’s architecture** such as when you decouple components using queues
[[Elastic Load Balancer Version]]