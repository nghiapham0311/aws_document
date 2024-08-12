ECS is capable of running in *EC2 mode* or *Fargate mode*.

*EC2 mode* deploys *EC2 instances* into your AWS account which can be used to deploy tasks and services.
With EC2 mode you pay for the EC2 instances regardless of container usage

*Fargate mode* uses shared AWS infrastructure, and ENI's which are injected into your VPC
You pay only for container resources used while they are running.
#### ECS - EC2 Mode
![[ECS Cluster Type.png]]
With EC2 mode, an ECS cluster is created within a **VPC inside your AWS account**. Because an EC2 mode cluster runs within a VPC, it benefits from the *multiple AZ*, which are available within this VPC.

So with EC2 cluster mode you are *responsible* for these *EC2 instances* that are acting as container *hosts*

So ECS using EC2 mode is not a serverless solution, you need to *worry* about **capacity and availability** for your cluster.

> It’s important to understand that with EC2 mode, even if you **aren’t running any tasks** or **any services** on your EC2 container hosts, you are **still paying for them while they’re in a running state**
#### ECS - Fargate Mode
With Fargate mode, you *don’t have to manage EC2 instances* for use as container hosts. Fargate is a *cluster model*, which means you have **no service to manage**. Because of this, you **aren’t paying for EC2 instances**, regardless of whether you’re using them or not.

What differs is how containers are actually hosted.

So if the VPC is *configured to use public subnets* which automatically allocate an IPv4 address, then *tasks and services can be given public IPv4*

Fargate offers a lot of *customizability* you can deploy exactly how you want into either a new VPC or a custom VPC that you have designed and implemented in AWS. With Fargate mode because tasks and services are *running from the shared infrastructure platform*, you only pay for the containers that you’re using based on the resources that they consume ⇒ So you have **no visibility of any host costs**, you **don’t need to manage hosts**, **provision hosts** or think about **capacity and availability**. That’s all handled by Fargate. You simply pay for the container resources that you consume.
## Exam Powerup
- If you use containers … **ECS**
- **Large** workload - **price** conscious - **EC2 Mode** pay for the container even if we arent use them
- **Large** workload - **overhead** conscious - **Fargate**
- **Small** / **Burst** workloads - **Fargate**
- **Batch** / **Periodic** workloads - **Fargate**

If you use containers, then pick ECS

If you’re a business which already uses containers for anything, then it makes sense to use ECS.

If you’re wanting to just quickly test containers, you can use EC2 as a Docker host, but for prod usage, it’s almost never a good idea

You would generally pick EC2 mode when you have a large workload and your business is price conscious. (gia re hon so voi fargate, fargate thi khong quan tam gi het cu day vao la xai)

If you care about price more than effort, you’ll want to look at using spot pricing or reserved pricing or make use of reservations that you already have

[[Kubernetes 101]] 