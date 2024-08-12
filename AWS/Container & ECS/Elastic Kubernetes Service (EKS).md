Amazon **Elastic Kubernetes Service** (Amazon EKS) is a fully-managed, **Kubernetes** implementation that simplifies the process of building, securing, operating, and maintaining **Kubernetes** clusters on AWS

- **AWS Managed Kubernetes** - open source & **cloud agnostic**
- … AWS, Outposts, EKS Anywhere, EKS Distro
- **Control plane scales** and runs on **multiple AZs**
- **Integrates** with **AWS services** … ECR, ELB, IAM, VPC
- **EKS Cluster** = EKS **Control Plane** & EKS **Nodes**
- **etcd** distributed across **multiple AZs**
- **Nodes** - **Self** Managed, **Managed node groups** or **Fargate** pods
- …windows, GPU, Inferentia, Bottlerocket, Outposts, Local zones… Check node type
- **Storage Providers** - include …EBS, EFS, FSx Lustre, FSx for NetApp ONTAP
![[Elastic Kubernetes Service (EKS).png]]

Conceptually, when you think of an EKS deployment, you’re going to have **2 VPCs**. The first is an *AWS managed VPC* and it’s here where the *EKS control plane will run* from *across multiple AZs*

The 2nd VPC is a *customer managed VPC*, in this case, the *Animals4life VPC*. If you’re going to be using EC2 worker nodes, then these will be deployed into the customer VPC.

Now, normally the control plane will communicate with these worker nodes via ENIs which are injected into the custom VPC. So the kubelet service running on the worker nodes connects to the control plane, either using these ENIs which are injected into the VPC, but it can also use a public control plane endpoint.

Any administration via the control plane can also be done using this public endpoint and any consumption of the EKS services is via ingress configurations, which start from the customer VPC.