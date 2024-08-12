- CloudWatch is for metrics
- CloudWatch Logs is for logging
- Neither natively capture **data inside an Instance**
- CloudWatch Agent is required…
- … plus **configuration** and **permissions**

In order for CloudWatch and CloudWatch Logs to have access inside of an EC2 instance, then there’s some configuration and security work required in addition to having to install the CloudWatch Agent

![[System and Application Logging on EC2.png]]

It’s *incapable* of injecting any logging into CloudWatch Logs *without* **the Agent being installed**. To fix that, we need to i*nstall the CloudWatch Agent* within the EC2 instance and the *Agent will need some configuration*. So it will need to know exactly what information to inject into CloudWatch and CloudWatch Logs

We need to supply the configuration information so the Agent knows what to do. *The Agent* also needs some way of interacting with *AWS on permissions*.

We know that it’s bad practice to add long-term credentials to an instance so we don’t want to do that, but that aside, it’s also difficult to manage that at scale.

The **best practice** for using this type of architecture is to **create an IAM role** with **permissions to interact with CloudWatch Logs and then we can attach this IAM role to the EC2 instance**, providing the instance or more specifically, anything running on the instance with access to the CloudWatch and CouldWatch Logs service.

Now the Agent configuration that we’ll also need to set up, that configures the metrics and the logs that we want to capture and these are all injected into CloudWatch using log groups. We’ll configure one log group for every log file that we want to inject into the product and then within each log group, there’ll be a log stream for each instance performing this logging.

So that’s the architecture, one log group for each individual log that we want to capture and then one log stream inside that log group for every EC2 instance that’s injecting that logging data.

[[EC2 Placement Group]]