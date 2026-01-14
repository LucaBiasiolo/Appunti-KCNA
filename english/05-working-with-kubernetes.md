# 05. Working with Kubernetes   
## Kubernetes Objects   
Kubernetes provides a lot of mostly abstract resources, also called **objects**.   
Kubernetes objects can be distinguished between **workload-oriented objects** that are used for handling container workloads and **infrastructure-oriented objects** to handle configuration, networking and security. Some of these objects can be put into a namespace, while others are available across the whole cluster.   
Objects are usually described in YAML language and sent to api-server, where they get validated before they are created.   
The required fields are:   
- **apiVersion**
Each object can be versioned. That means the data structure of the object can change between different versions.   
- **kind**
The kind of object that should be created.   
- **metadata**
Data that can be used to identify it. A **name** is required for each object and must be unique. You can use namespaces if you need multiple objects with the same name.   
- **spec**
The specification of the object. Here you can describe your desired state. **The structure of the object can change with its version!**   
   
   
## Interacting With Kubernetes   
To access the API, users can use the official command line interface client called **kubectl**.   
Despite the numerous CLI tools and GUIs, there are also advanced tools that allow the creation of templates and the packaging of Kubernetes objects. Probably the most frequently used tool in connection with Kubernetes today is [Helm](https://helm.sh/).   
**Helm is a package manager for Kubernetes, which allows easier updates and interaction with objects. Helm packages Kubernetes objects in so-called Charts, which can be shared with others via a registry.**   
   
## Pod Concept   
**The most important object in Kubernetes is a Pod. A pod describes a unit of one or more containers that share an isolation layer of namespaces and cgroups.** It is the smallest deployable unit in Kubernetes, which also means that Kubernetes is not interacting with containers directly. **All containers inside a pod share an IP address and can share via the filesystem**.   
You could add as many containers to your main application as you want. But be careful since you lose the ability to scale them individually! Using a second container that supports your main application is called a **sidecar container**.   
All containers defined are started at the same time with no ordering, but you also have the ability to use **initContainers** to start containers before your main application starts.   
   
## Pod Lifecycle   
Pods follow a defined lifecycle:   
- **Pending** the Pod has been accepted by the Kubernetes cluster, but one or more of the containers has not been set up and made ready to run. This includes time a Pod spends waiting to be scheduled, as well as the time spent downloading container images over the network   
- **Running** the Pod has been bound to a node, and all of the containers have been created. At least one container is still running, or is in the process of starting or restarting   
- **Succeeded** all containers in the Pod have terminated in success, and will not be restarted   
- **Failed** all containers in the Pod have terminated, and at least one container has terminated in failure. That is, the container either exited with non-zero status or was terminated by the system   
- **Unknown** for some reason, the state of the Pod could not be obtained. This phase typically occurs due to an error in communicating with the node where the Pod should be running   
   
   
## Workload Objects   
If a Pod is lost because a node failed, it is gone forever. To make sure that a defined number of Pod copies runs all the time, we can use controller objects that manage the pod for us.   
Kubernetes Objects:   
- **ReplicaSet** a controller object that ensures a desired number of pods is running at any given time. ReplicaSets ***can be used to scale out applications*** and improve their availability.   
- **Deployments** describe the complete application lifecycle, they do this by ***managing multiple ReplicaSets that get updated when the application is changed*** by providing a new container image, for example. Deployments are ***perfect to run stateless applications*** in Kubernetes   
- **StatefulSet** can be used to ***run stateful applications like databases***. Stateful applications have special requirements that don't fit the ephemeral nature of pods and containers. They try to retain IP addresses of pods and give them a stable name, persistent storage and more graceful handling of scaling and updates   
- **DaemonSet** ensures that a copy of a Pod runs on all (or some) nodes of your cluster. ***Perfect to run infrastructure-related workload***, for example monitoring or logging tools   
- **Job** creates one or more Pods that execute a task and terminate afterwards. Job objects are ***perfect to run one-shot scripts*** like database migrations or administrative tasks   
- **CronJob** add a time-based configuration to jobs. This ***allows running Jobs periodically***, for example doing a backup job every night at 4am   
   
   
## Networking Objects   
Since a lot of Pods would require a lot of manual network configuration, we can use *Service* and *Ingress objects* to define and abstract networking.   
Service Types:   
- **ClusterIP** is a virtual IP inside Kubernetes that ***can be used as a single endpoint for a set of pods***

   
![image](..files\image_x.png)    
- **NodePort** extends the ClusterIP by adding simple routing rules. It opens a port (default between 30000-32767) on every node in the cluster and maps it to the ClusterIP. This service type ***allows routing external traffic to the cluster***   
- **LoadBalancer** extends the NodePort by deploying an external LoadBalancer instance   
- **ExternalName** special service type that has no routing whatsoever. ExternalName is using the Kubernetes internal DNS server to create a DNS alias. You can use this to create a simple alias to resolve a rather complicated hostname. ***This is especially useful if you want to reach external resources from your Kubernetes cluster***   
- **Headless Services** sometimes you don't need load-balancing and a single Service IP. In this case, you can create what are termed "headless" Services, by explicitly specifying "None" for the cluster IP. ***You can use a headless Service to interface with other service discovery mechanisms, without being tied to Kubernetes' implementation***   
   
If you need even more flexibility to expose applications, you can use an *Ingress object*. ***Ingress provides a means to expose HTTP and HTTPS routes from outside of the cluster for a service within the cluster.*** It does this by configuring routing rules that a user can set and implement with an *ingress controller*.   
![image](..files\image_b.png)    
Kubernetes also provides a cluster internal firewall with the *NetworkPolicy* concept. NetworkPolicies are a simple IP firewall (OSI Layer 3 or 4) that can control traffic based on rules. You can define rules for incoming (ingress) and outgoing traffic (egress). A typical use case for NetworkPolicies would be restricting the traffic between two different namespaces.   
   
## Volume & Storage Objects   
Volumes allow sharing data between multiple containers within the same Pod. The second purpose they serve is preventing data loss when a Pod crashes and is restarted on the same node. Pods are started in a clean state, but all data is lost unless written to a volume.   
Kubernetes is using the [Container Storage Interface (CSI)](https://github.com/container-storage-interface/spec) which allows a storage vendor to write a plugin (storage driver) that can be used in Kubernetes.   
To use this abstraction, we have two more objects that can be used:   
- **PersistentVolumes (PV)**
***An abstract description for a slice of storage***. The object configuration holds information like type of volume, volume size, access mode and unique identifiers and information how to mount it.   
- **PersistentVolumeClaims (PVC)**
***A request for storage by a user***. If the cluster has multiple persistent volumes, the user can create a PVC which will reserve a persistent volume according to the user's needs.   
   
After the PersistentVolume is provisioned, a developer can reserve it with a PersistentVolumeClaim. The last step is using the PVC as a volume in a Pod.   
   
## Configuration Objects   
It is considered bad practice to incorporate the configuration directly into the container build. Any configuration change would require the entire image to be rebuilt and the entire container or pod to be redeployed.   
In Kubernetes, this problem is solved by decoupling the configuration from the Pods with a ConfigMap. ConfigMaps can be used to store whole configuration files or variables as key-value pairs. There are two possible ways to use a ConfigMap:   
- **Mount a ConfigMap as a volume in Pod**   
- **Map variables from a ConfigMap to environment variables of a Pod**   
   
Kubernetes also provides ***Secrets to store sensitive information like passwords, keys or other credentials***. Secrets are very much related to ConfigMaps and basically their only difference is that secrets are base64 encoded.   
   
## Autoscaling Objects   
To scale the workload in a Kubernetes cluster, we can use three different Autoscaling mechanisms:   
- **Horizontal Pod Autoscaler (HPA)** is the most used autoscaler in Kubernetes. The HPA ***can watch Deployments or ReplicaSets and increase the number of Replicas if a certain threshold is reached***   
- **Cluster Autoscaler** of course, there is no point in starting more and more Replicas of Pods, if the Cluster capacity is fixed. The Cluster Autoscaler ***can add new worker nodes to the cluster if the demand increases***. The Cluster Autoscaler works great in tandem with the Horizontal Autoscaler   
- **Vertical Pod Autoscaler** ***allows Pods to increase the resource requests and limits dynamically***. Vertical scaling is limited by the node capacity   
   
Horizontal autoscaling in Kubernetes is NOT available out of the box and requires installing an add-on called [metrics-server](https://github.com/kubernetes-sigs/metrics-server).   
[KEDA](https://keda.sh/) (**Kubernetes-based Event Driven Autoscaler**) can be used to scale the Kubernetes workload based on events triggered by external systems. Similar to the HPA, KEDA can scale deployments, ReplicaSets, pods, etc., but also other objects such as Kubernetes jobs. With a large selection of out-of-the-box scalers, KEDA can scale to special triggers such as a database query or even the number of pods in a Kubernetes cluster.   
   
## Scheduling Objects   
The scheduler is the control process which assigns Pods to Nodes. The scheduler determines which Nodes are valid placements for each Pod in the scheduling queue according to constraints and available resources. The scheduler then ranks each valid Node and binds the Pod to a suitable Node. ***kube-scheduler is the default implementation***.   
There are scenarios where you want to restrict the pod on particular nodes or prefer to run on particular nodes, main methods are:   
- **nodeSelector field matching against node labels** you can add the nodeSelector field to your Pod specification and specify the node labels you want the target node to have. ***Kubernetes only schedules the Pod onto nodes that have each of the labels you specify***   
- **Affinity and anti-affinity** you can indicate that a rule is soft or preferred, so that the ***scheduler still schedules the Pod even if it can't find a matching node***   
- **nodeName field** nodeName is a field in the Pod spec; if is not empty, ***the scheduler ignores the Pod and the kubelet (on the named node) tries to place the Pod on that node, overriding nodeSelector or affinity and anti-affinity rules***   
- **Pod topology spread constraints** you can use topology spread constraints to ***control how Pods are spread across your cluster among failure-domains such as regions, zones, nodes***, or among any other topology domains that you define   
   
### Taints and tolerations   
Node affinity is a property of Pods that attracts them to a set of nodes (either as a preference or a hard requirement). ***Taints are the opposite — they allow a node to repel a set of pods***.   
Tolerations are applied to pods. Tolerations allow the scheduler to schedule pods with matching taints. Tolerations allow scheduling, but don't guarantee scheduling: the scheduler also evaluates other parameters as part of its function.   
***Taints and tolerations work together to ensure that pods are not scheduled onto inappropriate nodes.***   
A taint consists of a key, value, and effect. As an argument here, it is expressed as **key=value:effect**.   
   
## Kubernetes Security   
Because Kubernetes is a network-intensive environment, it is crucial to secure the network using typical firewall techniques from outside the Kubernetes cluster and using pod-to-pod encryption, a NetworkPolicy and other measures from within the Kubernetes cluster.   
### Accessing the API   
To perform any action in a Kubernetes cluster, you need to access the API and go through three main steps.   
### 1. Authentication (token)   
The type of authentication used is defined in the kube-apiserver startup options, the following are some examples of authentication mechanism:   
```
--basic-auth-file

--oidc-issuer-url

--token-auth-file

--authorization-webhook-config-file
```
### 2. Authorization (RBAC)   
RBAC stands for Role Based Access Control. All resources are modeled API objects in Kubernetes, from Pods to Namespaces. They also belong to API Groups such as **core** and **apps**. These resources allow operations such as Create, Read, Update, and Delete (CRUD), which we have been working with so far.   
In YAML files, operations are referred to as verbs.   
Rules are operations which can act upon an API group.   
Roles are a group of rules which affect, or scope, a single namespace, whereas ClusterRoles have a scope of the entire cluster.   
Each operation can act upon one of three subjects: User Accounts, which don’t exist as API objects; Service Accounts, and Groups, which are known as clusterrolebinding when using **kubectl**.   
### 3. Admission Controllers   
Admission controllers are pieces of software that can access the content of the objects being created by the requests. They can modify the content or validate it, and potentially deny the request.   
