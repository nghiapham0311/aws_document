The EC2 Instance Metadata is a service that EC2 provides to instances. It’s *data* about the instance that can be *used to configure or manage a running instance*. It’s a way the *instance* or *anything* running inside the *instance* can *access* **information about the environment that it wouldn’t be able to access otherwise**

It’s accessible inside all instances using the same access method

The IP address to access the Instance Metadata is `169.254.169.254`

> **IMPORTANT: Remember 169.254.169.254 - [http://169.254.169.254/latest/meta-data**](http://169.254.169.254/latest/meta-data**)

Metadata allows anything running on the instance to query it for information about that instance and that information is divided into categories

- Host name
- Events
- Security groups
- Environment

> **IMPORTANT: It has no authentication and it’s not encrypted**

Anyone who can connect to an instance and gain access to the Linux command line shell can by default access the Metadata

In general, you should treat the Metadata as something that can and does get exposed