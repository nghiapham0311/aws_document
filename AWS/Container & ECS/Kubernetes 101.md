![[Kubernetes 101.png]]
Kubernetes is a cloud agnostic product. So you can use it on premises and within many public cloud platforms

A cluster in Kubernetes is a highly available cluster of compute resources, and these are organized to work as one unit.

The cluster starts with the cluster control plan, which is the part which manages the cluster. It performs scheduling, application management, scaling and deployment, and much more.

Compute within a Kubernetes cluster is provided via nodes, and these are virtual or physical servers which function as a worker within the cluster. These are the things which actually run your containerized applications.

Running on each of the nodes is software and at minimum, this is containerd or another container run time, which is the software used to handle your container operations.

Next, we have kubelet, which is an agent to interact with the cluster control plane. Kubelets running on each of the nodes communicates with the cluster control plan using the Kubernetes API. This is the top level functionality of the Kubernetes cluster.

The control plane orchestrates containerized applications, which run on nodes.

#### Cluster Detail
![[Kubernetes 101-1.png]]

We have the control plane at the top and a single cluster node at a bottom complete with the minimum Docker and kubelet software running for control plane communications.

The cluster will also likely have many more nodes. It’s rare that you only have one node unless this is a testing environment.

Pods are the smallest unit of computing within Kubernetes. You can have pods which have multiple containers and provide shared storage and networking for those pods. But it’s very common to see a one container, one pod architecture, which, as the name suggests, means each pod contains only one container.

> When you think about Kubernetes, don’t think about containers, think about pods

The pods handle the containers within them. Architecturally, you would generally only run multiple containers in a pod when those containers are tightly coupled and require close proximity and rely on each other in a very tightly coupled way. Additionally, although you’ll be exposed to pods, you’ll rarely manage them directly. Pods are non-permanent things. In order to get the maximum value from Kubernetes, you need to view pods as temporary things, which are created, do a job and are then disposed of.

Pods can be deleted when finished, evicted for lack of resources, or if the node itself fails. They aren’t permanent and aren’t designed to be viewed as highly available entities.

What’s run on the control plane

1. The API
    
    known formally as kube-apiserver. This is the front-end for the control plane. It’s what everything generally interacts with to communicate with the control plane. And it can be scaled horizontally for performance and to ensure high availability.
    
2. The etcd
    
    This provides a highly-available key value store. So a simple database running within the cluster, which acts as the main backing store for data for the cluster.
    
3. The kube-scheduler
    
    This is responsible for constantly checking for any pods within the cluster, which don’t have a node assigned. And then it assigns a node to that pod based on resource requirements, deadlines, affinity or anti-affinity, data locally needs and any other constraints.
    
    > Remember, nodes are the things which provide the raw compute and other resources to the cluster
    
4. The cloud-controller-manager (optional)
    
    This is what allows Kubernetes to integrate with any cloud providers. It’s common that Kubernetes runs on top of other cloud platforms, such as AWS, Azure or GCP. This component which allows the control plan to closely interact with those platforms
    
5. The kube-controller-manager
    
    This is actually a collection of processes.
    
    We’ve got the node controller, which is responsible for monitoring and responding to any node outages.
    
    The job controller, which is responsible for running pods in order to execute jobs.
    
    The endpoint controller, which populates endpoints in the cluster, this is something that links services to pods
    
    The service account and token controller which is responsible for account and API token creation.
    

Lastly, on every node is something called K proxy, known as kube-proxy. This runs on every node and coordinates networking with the cluster control plane. It helps implement services and configures rules, allowing communications with pods from inside or outside of the cluster. You might have a Kubernetes cluster but you’re going to want some level of communication with the outside world

## Summary
- **Cluster** - A deployment of Kubernetes, management, orchestration…
- **Node** - Resources; pods are placed on nodes to run
- **Pod** - 1+ Containers; smallest unit in kubernetes, often 1 container 1 pod
- **Service** - Abstraction, service running on 1 or more pods
- **Job** - ad-hoc, creates one or more pods until completion
- **Ingress** - Exposes a way into a service (**Ingress ⇒ Routing ⇒ Service ⇒ 1+ Pods**)
- **Ingress Controller** - used to provide ingress (e.g AWS LB Controller uses ALB/NLB)
- **Persistent** Storage (**PV**) - Volume whose lifecycle lives beyond any 1 pod using it

Service provide an abstraction from pods. So the service is typically what you will understand as an application. An application can be containerized across many pods, but the service is the consistent thing, the abstraction. Service is what you interact with if you access a containerized application

Ingress is how something external to the cluster can access a service. So you have external users. They come into an ingress that’s routed through the cluster to a service. The service points at one or more pods, which provides the actual application. ⇒ An ingress is something that you will have exposure to when you start working with Kubernetes

If your application has any form of long-running state, then you need to wait to store that state somewhere. Now, state can be session data but also data in the more traditional sense. Any storage in Kubernetes by default is ephemeral, provided locally by a node. And thus, if a pod moves between nodes, then that storage is lost.

Now you can configure persistent storage, and these are volumes whose life cycle lives beyond any one single pod, which is using them. This is how you would provision normal, long-running storage to your containerized applications

[[Elastic Kubernetes Service (EKS)]]