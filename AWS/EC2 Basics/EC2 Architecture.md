- EC2 instances are **virtual machines** (OS + resources)
- EC2 instances run on **EC2 Hosts**
- **Shared** Hosts or **Dedicated** Hosts (Dùng chung cục server to với thằng khác hoặc có riêng một server to phục vụ EC2 của ta)
- **Hosts = 1 Az** - AZ Fails, Host Fails, Instance Fail -> **AZ resilient**
![[EC2 Architecture.png]]
EC2 hosts have some local hardware, logically CPU and memory which you should be aware of, but also, they have some *local storage called the instance store*

The Instance Store is *temporary*. If an instance is running on a particular host depending on the type of the instance it might be able to utilize this Instance Store. But if the instance *moves off this host to another one* then that *storage is lost*

They also have two types of networking, **storage networking and data networking**. When *instances are provisioned into a specific subnet* within a VPC, what’s actually happening is that a *primary elastic network interface is provisioned in a subnet which maps to the physical hardware on the EC2 host*.

> Subnets are also in one specific AZ

Instances can have **multiple network interfaces even in different subnets**, as long as they’re **in the same AZ**

EC2 can make use of remote storage, so an EC2 host can connect to elastic block store, which is known as EBS

The *elastic block store* service also runs **inside a specific AZ**. So the service running inside AZ-A is different than the one running inside AZ-B and you can’t access them cross zone

EBS lets you allocate volumes and volumes of portions of persistent storage. These can be allocated to instances in the same AZ
- The host is in AZ
- The network is per AZ
- The persistent storage is per AZ
If an AZ experience its major issues it impacts all those things

Instances stay on a host until one of two things happen:
- The host fails or it taken down for maintenance for some reason by AWS
- If an instance is stopped and then started and that’s different than just restarting

Instances *can not natively move between AZ*, everything about them, their hardware, networking and storage is locked inside one specific AZ

What you **can never do is connect network interfaces or EBS storage located in one AZ to an EC2 instance located in another**.

> EC2 and EBS are both AZ services. They’re isolated

Instances of different sizes can share a host but generally, instances of the same type and generation will occupy the same host.
#### What’s EC2 Good for?
- Traditional **OS + Application** Compute
- **Long Running** Compute => co 1 app running 24/7
- **Server** style applications
- .. either **burst** or **steady-state** load
- Great for **Monolithic** application stacks
- **Migrated** application workloads or **Disaster Recovery**
=> Pick EC2 as the *default* and move away if need some specific requirements.

[[EC2 Instance Types]]