![[RDS Data Security.png]]
With all of the different engines within RDS, you can use *encryption in transit* which means *data between the client and the RDS instance is encrypted via SSL or TLS*. This can actually be set to mandatory on per user basis

Encryption at rest is supported in a few different ways depending on the db engine. By default, it’s supported *using KMS and EBS encryption*, so this is *handled by* the **RDS host** and the **underlying EBS-based storage**.

As far as the RDS db engine knows, it’s just writing unencrypted data to storage. The data is encrypted by the host that the RDS instance is running on.

KMS is used, and so you select a customer master key, or CMK, to use either a customer-managed CMK or an AWS-managed CMK. And then this CMK is used to generate data encryption keys, or DEKs, which are used for the actual encryption operations.

When using this type of encryption, the *storage, the logs, the snapshots and any replicas are all encrypted* using the *same customer master key*.

**Encryption cannot be removed once it’s added**

[[Aurora Architect]]