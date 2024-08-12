*Elastic Container Service*
ECS is a product which *allows* you to use containers running on *infrastructure*, which AWS *fully manage or partially manage*.

> ECS is a service that *accepts containers* and *some instructions* that you provide and it *orchestrates* **where** and how **to** run those containers. It’s a managed container based compute service.

ECS two modes:
- `EC2 mode`: EC2 mode which **uses EC2 instances as container hosts**, and you can see these inside your account. They’re just normal EC2 hosts running the ECS software.
- `Fargate mode`: which is a **serverless** way of running docker containers where **AWS manage the container host part** and just leave *you to define an architect* to your environment using containers.
![[ECS Concept.png]]
You *provide* ECS with a *container image* and it **runs** that in the form of a container in the cluster based on how you want it to run.

*Task* in ECS represents a **self-contained application**. A task can include *one or more* containers

*Task definitions* store the *resources* used by the task, so *CPU and memory*, they store the *networking mode* that the task uses, the store the compatibility.

> A task role is an IAM role that a task can assume. And when the task assumes that role, it gains temporary credentials, which can be used within the task to interact with AWS resources.

**Task roles** are the **best practice way** of giving containers within ECS **permissions** to access AWS products and services

*A service definition* defines a **service** and a **service** is how for ECS, we can define how we want a **task to scale, how many copies we’d like to run**. It can add *capacity* and it can add *resilience* because we can have multiple independent *copies* of *our task running* and you can *deploy a load balancer in front of a service*. ⇒ So incoming load is distributed across all of the tasks inside a service. So for tasks that you’re running inside ECS, the long running and business critical, you would generally use a **service** to **provide that level of scalability and high availability.**
## ECS Concepts
- **Container Definition** - Image & Ports
- **Task Definition** - Security (Task Role), Container(s), Resources
- **Task Role** - IAM Role which the TASK assumes
- **Service** - How many copies, HA, Restarts

[[ECS Cluster Type]]