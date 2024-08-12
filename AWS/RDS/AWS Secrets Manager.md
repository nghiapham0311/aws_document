*AWS Secrets manager* is a product which can **manage secrets within AWS**. There is some *overlap between it and the SSM Parameter Store* - but Secrets manager is **specialised for secrets**.

Additionally Secrets managed is **capable of automatic credential rotation** using **Lambda**.

For supported services it can even adjust the credentials of the service itself.
![[AWS Secrets Manager.png]]

It shares functionality with Parameter Store, you can use either an achieve the same result.

Secret Manager, though, is designed specifically for secrets, so this means things like passwords and API keys.

Secret Manager is usable from the Console, the CLI, the API, and software development kits. It’s actually designed architecturally to be integrated inside other applications.

Another key differentiator of Secrets Manager is that it actually supports the automatic rotation of secrets. This uses Lambda. Essentially, a Lambda function is invoked periodically and it’s used to update the secrets and for certain AWS products, such as RDS, Secrets Manager supports direct integration. So as well as being able to periodically change a secret that’s stored inside Secrets Manager, the product can also make sure that any authentication built into that product, such as RDS, is also changed so it’s kept synchronized with Secrets Manager. So if Lambda is invoked and changes a secret, so rotates a secret, then the password inside an RDS instance can also be changed, and that integration is only supported for a certain limited set of products

This product is specifically designed and focuses on the storage and rotation of these secrets. So the product keeps them safe, they’re encrypted at rest, to control access to the secrets, it rotates them and clients can use Secrets Manager to access the secrets and then use these to communicate with, a db in a safe and secure way.

![[AWS Secrets Manager-1.png]]

The SDK uses IAM credentials for authorization, generally a role, but it might also use access keys, even though this is less ideal.

These credentials are used to interact with Secrets Manager and retrieve the secrets for the db that the application uses. So once the application has these secrets, it can use them to securely access the db.

**What sets the Secrets Manager apart is that, periodically, the Secrets Manager can invoke a Lambda function to rotate credentials**. The Lambda function will require permissions to do this and it gets those permissions from an execution role and it will use these permissions both to update the secret that is stored within Secrets Manager, but also, if you use supported products, such as RDS, then the actual authentication information inside RDS can also be updated and kept in sync with the secrets that are inside the Secret Manager product

If you’re using a supported, integrated product, then everything is managed end-to-end by Secrets Manager. It can handle the rotation of credentials, it can handle the update of products which use those credentials, and as long as the application keeps checking in with Secrets Manager, it can always ensure that it has access to the most updated versions of those secrets

That secrets are secured using KMS so you never risk any leakage via physical access to the AWS hardware, and KMS also ensure role separation, which means that you need permissions both to KMS and to Secrets Manager in order to access secrets and decrypt them.
## Exam Powerup

> If you see **any questions** where you **suspect that Secrets Manager** might be involved, you need to do **keyword analysis**. So you need to determine if the question **mentions** anything in the **area of secrets**, you need to check if the question mentions **anything to do with rotation** and if it mentions either of them or both of them and if it **also mentions a product such as RDS** then you’re almost certain to be using **Secrets Manager**

[[RDS Proxy]]