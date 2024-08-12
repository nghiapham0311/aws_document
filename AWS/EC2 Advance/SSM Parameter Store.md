Passing *secret* data *into EC2 instance* using user-data is a **bad practice** -> SSM Parameter is **best practice**

The SSM Parameter store is a *service which is part of Systems Manager* which **allows the storage and retrieval of parameters** - string, stringlist or secure string.

The service supports *encryption* which integrates with *KMS*, *versioning* and can be secured using *IAM*.

The service integrates natively with many AWS services - and can be accessed using the CLI/APIs from anywhere with access to the AWS Public Spare Endpoints.

- Storage for **configuration** & **secrets**
- String, StringList & SecureString
- License codes, Database Strings, Full Configs & Passwords
- Hierarchies & Versioning
- Plaintext and Ciphertext
- Public Parameters - **Latest AMIs per region**

Parameter store also allows you to store parameters using a *hierarchical structure* (Like /wordpress/ in below image). Parameter store also *stores different versions of parameters*, so just like we’ve got object versioning in S3, inside parameter store we can also have different versions of parameters.

The parameter stores also got the concept of *public parameters* so these are parameters *publicly available and created by AWS*

![[SSM Parameter Store.png]]
Now the architecture of the parameter store is simple enough to understand that it’s a *public service*, so anything *using it* needs to *either be an AWS service or have access to the AWS public space and points*.

Different types of things can use the parameter store. EC2 instances, all the things running on those instances, and even Lambda functions. They can all request access to parameters inside the parameter store.

As parameters store is an AWS service, it’s **tightly integrated with IAM** for permissions, so any **accesses will need to be authenticated an authorized** and that might use *long-term credentials*.

So access keys, all those credentials **might be passed in via an IAM role**. And if parameters are *encrypted* then *KMS will be involved*, and the appropriate permissions to the CMK inside KMS will also be required.

Parameter store allows you to create simple or complex set of parameters. You can also create hierarchical structure. So something like `/WordPress/`

Now permissions are flexible, and they can be set either on individual parameters or whole trees. Everything supports versioning and any changes that occur to any parameters can also spawn events. And these events can start off processes in other AWS services

[[System and Application Logging on EC2]]
