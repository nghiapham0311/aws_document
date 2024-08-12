Bootstrapping is a *process where scripts or other bits of configuration can be run* when an instance is **first launched**, meaning that an instance can be brought into service in a certain pre-configured state.

So unlike just launching an instance with an AMI and having it be in its default state, we can bootstrap in a certain set of configurations or software installs.
#### EC2 Boostrapping
- Bootstrapping allows **EC2 Build Automation**
- User Data - Accessed via the meta-data IP
- **[http://169.254.169.254/latest/user-data**](http://169.254.169.254/latest/user-data**)
- Anything in User Data is **executed** by the **instance OS**
- **ONLY on Launch**
- EC2 doesn’t interpret, the OS needs to understand the User Data

It’s accessed using the metadata IP address. So 169.254.169.254. But instead of **latest/meta-data, it’s latest/user-data**

> It’s executed only **ONCE** at *launch time*. If you update the user data and restart an instance, it’s not executed again.

![[Bootstrap EC2 Using User Data.png]]

AMI is used to launch an EC2 instance in the usual way and this creates an EBS volume, which is attached to the EC2 instance. Where it starts to differ is that now the EC2 service provides some user data through to the EC2 instance. And there’s software within the OS running on EC2 instances, which is designed to look at the metadata IP for any user data.

And if it sees any user data, then it executes this on launch of that instance. Now this user data is treated just like any other script that the OS runs. **It needs to be valid and at the end of running the script, the EC2 instance will either be in a running state and ready for service, meaning that the instance has finished its startup process, the user data ran and it was successful and the instance is in a functional and running state**

Or the worst case is that the user data errors in some way. **So the instance would still be in a running state**. Because the user data is separate from EC2, EC2 just deliver it into the instance.

The instance *would still pass it status checks* and assuming you didn’t run anything which deleted mass amounts of OS data, you could probably still connect to it. But the instance would likely not be configured as you want. It would be a bad configuration
#### Boot-Time-To-Service-Time
![[Bootstrap EC2 Using User Data-1.png]]
You can shorten this post launch time in a few ways. Alternatively, you can also also do the work in advance by AMI baking. With this method, you’re front loading the work, doing it in advance and creating an AMI with all of that work baked in. This removes the post launch time but it means you can’t be as flexible with the configuration because it has to be baked into the AMI

The optimal way is to *combine both of these processes*. So *AMI baking* and *bootstrapping*. You’d use *AMI baking* for any part of the process, which is **time intensive**. So if you have an application installation process, which is 90% installation and 10% configuration, you can AMI bake in the 90% part and then bootstrap the final configuration.

That way, you reduce the post launch time and thus the boot-time-to-service-time but you also get to use bootstrapping, which gives you much more configurability.
## Exam Powerup
- It’s **opaque** to EC2 .. its just a block of data
- It’s **NOT** secure - don’t use it for passwords or long term credentials (ideally)
- User data is limited to **16KB in size**
- Can be modified when instance stopped
- But **only executed once at launch**

The user data is not secure, anyone who can access the instance OS can access the user data. So don’t use it for passing in any long-term credentials.

> The user data is generally used the once for the post-launch configuration of an instance. It’s only executed the one initial time.

[[EC2 Instance Role]]

