# 04. Kubernetes Fundamentals   
## Kubernetes Architecture   
Kubernetes clusters consist of two different server node types that make up a cluster:   
- **Control plane node(s)**
These are the brains of the operation. Control plane nodes contain various components which manage the cluster and control various tasks like deployment, scheduling and self-healing of containerized workloads.   
- **Worker nodes**
The worker nodes are where applications run in your cluster. This is the only job of worker nodes and they don’t have any further logic implemented. Their behavior, like if they should start a container, is completely controlled by the control plane node.   
   
Control plane nodes typically host the following services:   
- **kube-apiserver** all other components interact with the api-server and this is where users access the cluster   
- **etcd** a database which holds the state of the cluster   
- **kube-scheduler** when a new workload should be scheduled, the kube-scheduler chooses a worker node that could fit, based on different properties like CPU and memory   
- **kube-controller-manager** contains different non-terminating control loops that manage the state of the cluster   
- **cloud-controller-manager** (optional) can be used to interact with the API of cloud providers, to create external resources like load balancers, storage or security groups   
   
Components of worker nodes:   
- **container runtime** is responsible for running the containers on the worker node   
- **kubelet** is a small agent that runs on every worker node in the cluster, talks to the api-server and the container runtime to handle the final stage of starting containers   
- **kube-proxy** is a network proxy that handles inside and outside communication of your cluster, trying to rely on the networking capabilities of the underlying operating system if possible   
   
***It is important to note that this design makes it possible that applications that are already started on a worker node will continue to run even if the control plane is not available. Although a lot of important functionality like scaling, scheduling new applications, etc., will not be possible while the control plane is offline.***   
Kubernetes also has a concept of *namespaces*, which are not to be confused with kernel namespaces that are used to isolate containers. **A Kubernetes namespace can be used to divide a cluster into *multiple virtual clusters***, which can be used for multi-tenancy when multiple teams share a cluster.   
![image](..files\image.png)    
   
## Kubernetes Setup   
Creating a test "cluster" can be very easy with:   
- [Minikube](https://minikube.sigs.k8s.io/docs/)   
- [kind](https://kind.sigs.k8s.io/)   
- [MicroK8s](https://microk8s.io/)   
   
Set up a production-grade cluster on your own hardware or virtual machines   
- [kubeadm](https://kubernetes.io/docs/reference/setup-tools/kubeadm/)   
- [kops](https://github.com/kubernetes/kops)   
- [kubespray](https://github.com/kubernetes-sigs/kubespray)   
   
Kubernetes packaged into a distribution:   
- [Rancher](https://rancher.com/)   
- [k3s](https://k3s.io/)   
- [OpenShift](https://www.redhat.com/en/technologies/cloud-computing/openshift)   
- [VMWare Tanzu](https://tanzu.vmware.com/tanzu)   
   
Cloud providers:   
- [Amazon (EKS)](https://aws.amazon.com/eks/)   
- [Google (GKE)](https://cloud.google.com/kubernetes-engine)   
- [Microsoft (AKS)](https://azure.microsoft.com/en-us/services/kubernetes-service)   
- [DigitalOcean (DOKS)](https://www.digitalocean.com/products/kubernetes/)   
   
   
## Kubernetes API   
Without it, communication with the cluster is not possible, every user and every component of the cluster itself needs the api-server.   
**The Kubernetes API is implemented as a RESTful interface that is exposed over HTTPS.**   
Before a request is processed by Kubernetes, it has to go through three stages:   
- **Authentication**
The requester needs to present a means of identity to authenticate against the API. Commonly done with a digital signed certificate ( [X.509](https://en.wikipedia.org/wiki/X.509)) or with an external identity management system. Kubernetes users are *always* externally managed. [Service Accounts](https://kubernetes.io/docs/reference/access-authn-authz/service-accounts-admin/) can be used to authenticate technical users.   
- **Authorization**
It is decided what the requester is allowed to do. In Kubernetes this can be done with [Role Based Access Control (RBAC)](https://kubernetes.io/docs/reference/access-authn-authz/rbac/).   
- **Admission Control**
In the last step, [admission controllers](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/) can be used to modify or validate the request. For example, if a user tries to use a container image from an untrustworthy registry, an admission controller could block this request. Tools like the [Open Policy Agent](https://www.openpolicyagent.org/) can be used to manage admission control externally.   
![image](..files\image_3.png)    
   
   
## Running Containers on Kubernetes   
In Kubernetes, instead of starting containers directly, you **define Pods as the smallest compute unit** and Kubernetes translates that into a running container.   
![image](..files\image_a.png)    
Container Runtimes:   
- **containerd** is a lightweight and performant implementation to run containers   
- **CRI-O** was created by Red Hat   
- **Docker** the standard for a long time, but never really made for container orchestration (removed from Kubernetes 1.24)   
   
containerd and CRI-O try to solve the security problem that comes with sharing the kernel between multiple containers. The most common tools at the moment are:   
- [gvisor](https://github.com/google/gvisor)
Made by Google, provides an application kernel that sits between the containerized process and the host kernel.   
- [Kata Containers](https://katacontainers.io/)
A secure runtime that provides a lightweight virtual machine, but behaves like a container.   
   
   
## Networking   
Containers need to communicate across a lot of nodes. Kubernetes distinguishes between four different networking problems that need to be solved:   
1. **Container-to-Container communications**
This can be solved by the Pod concept as we'll learn later.   
2. **Pod-to-Pod communications**
This can be solved with an overlay network.   
3. **Pod-to-Service communications**
It is implemented by the kube-proxy and packet filter on the node.   
4. **External-to-Service communications**
It is implemented by the kube-proxy and packet filter on the node.   
   
There are different ways to implement networking in Kubernetes, but also three important requirements:   
- All pods can communicate with each other across nodes.   
- All nodes can communicate with all pods.   
- No Network Address Translation (NAT).   
   
In Kubernetes, **every Pod gets its own IP address**, so there is no manual configuration involved. Moreover, most **Kubernetes setups include a DNS server add-on called [core-dns](https://kubernetes.io/docs/tasks/administer-cluster/coredns/)**, which can provide service discovery and name resolution inside the cluster.   
Every pod can communicate with other pods on the Kubernetes cluster, however if you want to control the traffic flow at the IP address or port level, then you have to use Network Policies.   
   
## Scheduling   
Describes the process of automatically choosing the right (worker) node to run a containerized workload on.   
The kube-scheduler is the component that makes the scheduling decision, but is not responsible for actually starting the workload (it will get started by kubelet and container runtime). The scheduling process in Kubernetes always starts when a new Pod object is created.   
A user has to give information about the application requirements, including requests for CPU and memory and properties of a node.   
The scheduler will use that information to filter all nodes that fit these requirements. If multiple nodes fit the requirements equally, Kubernetes will schedule the Pod on the node with the least amount of Pods.   
If worker nodes do not have sufficient resources to run your application, the scheduler will retry to find an appropriate node until the state can be established.   
