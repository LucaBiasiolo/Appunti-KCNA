# 03. Container Orchestration   
## Use of Containers   
While the developer knows his application and its dependencies best, it is usually a system administrator who provides the infrastructure, installs all of the dependencies, and configures the system on which the application runs. This process can be very error-prone and hard to maintain, so servers are only configured for a single purpose like running a database or an application server and then get connected by network.   
Virtual machines can be used to emulate a full server with cpu, memory, storage, networking, operating system and the software on top. This allows running multiple isolated servers on the same hardware. But since you have to run a whole operating system including the kernel, ***it always comes with some overhead*** if you need to run a lot of servers.   
***Containers can be used to solve both of these problems, managing the dependencies of an application and running much more efficiently than spinning up a lot of virtual machines.***   
   
## Container Basics   
Contrary to popular belief, container technologies are much older than one would expect. One of the earliest ancestors of modern container technologies is the **chroot** command that was introduced in Version 7 Unix in 1979. The **chroot** command could be used to isolate a process from the root filesystem and basically "hide" the files from the process and simulate a new root directory. The isolated environment is a so-called *chroot jail*, where the files can’t be accessed by the process, but are still present on the system.   
**To isolate a process even more than chroot can do, current Linux kernels provide features like *namespaces* and cgroups.**   
**Namespaces** are used to isolate various resources, for example the network. A network namespace can be used to provide a complete abstraction of network interfaces and routing tables.   
The Linux Kernel 5.6 currently provides 8 namespaces:   
- **pid** - process ID provides a process with its own set of process IDs.   
- **net** - network allows the processes to have their own network stack, including the IP address.   
- **mnt** - mount abstracts the filesystem view and manages mount points.   
- **ipc** - inter-process communication provides separation of named shared memory segments.   
- **user** - provides process with their own set of user IDs and group IDs.   
- **uts** - Unix time sharing allows processes to have their own hostname and domain name.   
- **cgroup** - a newer namespace that allows a process to have its own set of cgroup root directories.   
- **time** - the newest namespace can be used to virtualize the clock of the system.   
   
**cgroups** are used to organize processes in hierarchical groups and assign them resources like memory and CPU.   
***While virtual machines emulate a complete machine, including the operating system and a kernel, containers share the kernel of the host machine and, as explained, are only isolated processes.***   
Virtual machines come with some overhead, be it boot time, size or resource usage to run the operating system. ***Containers on the other hand are literally processes***, like the browser you can start on your machine, therefore they start a lot faster and have a smaller footprint.   
   
## Running Containers   
To run industry-standard containers, you don't need to use Docker; you can just follow the OCI [runtime-spec](https://github.com/opencontainers/runtime-spec) standard instead.   
The Open Container Initiative also maintains a container runtime reference implementation called [runC](https://github.com/opencontainers/runc).   
For your local machine, there are plenty of alternatives to choose from, some of which are only for building images like [buildah](https://buildah.io/) or [kaniko](https://github.com/GoogleContainerTools/kaniko), while others line up as full alternatives to Docker, like [podman](https://podman.io/) does.   
   
## Building Container Images   
Why is a container called a container in the first place? The metaphor used here is aimed at the use of shipping containers that are standardized according to [ISO 668](https://en.wikipedia.org/wiki/ISO_668).   
Docker reused all components to isolate processes like namespaces and cgroups, but a crucial piece of the puzzle that helped containers achieve their breakthrough is the introduction of *container images. *Docker describes a container image as following:   
"*A Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings."*   
Images consist of a filesystem bundle and metadata.   
Images can be built by reading the instructions from a buildfile called *Dockerfile***.**    
   
## Security   
When containers are started on a machine, they always share the same kernel, which then becomes a risk for the whole system.   
***One of the greatest security risks is the execution of processes with too many privileges***, especially starting processes as root or administrator.   
The ***4C's of Cloud Native security*** can give a rough idea what layers need to be protected if you’re using containers:   
- **Code**   
- **Container**   
- **Cluster**   
- **Cloud **(/Co-Lo/Corporate Datacenter)   
   
   
## Container Orchestration Fundamentals   
Having a lot of small containers that are loosely coupled, isolated and independent is the basis for so-called microservice architectures.   
If you have to manage and deploy large amounts of containers, you quickly get to the point where you need a system that helps with the management of these containers. Problems to be solved can include:   
- **Providing compute resources** like virtual machines where containers can run on   
- **Schedule** containers to servers in an efficient way   
- **Allocate resources** like CPU and memory to containers   
- **Manage the availability** of containers and replace them if they fail   
- **Scale** containers if load increases   
- **Provide networking** to connect containers together   
- **Provision storage** if containers need to persist data   
   
Most container orchestration systems consist of **two parts**: a ***control plane that is responsible for the management of the containers*** and ***worker nodes that actually host the containers***.   
   
## Networking   
Network namespaces allow each container to have its own unique IP address, therefore multiple applications can open the same network port.   
To make the application accessible from outside the host system, containers have the ability to map a port from the container to a port from the host system.   
To allow communication between containers across hosts, we can use an overlay network which puts them in a virtual network that is spanned across the host systems.   
Most modern implementations of container networking are based on the Container Network Interface (CNI).   
![image](..\files\image_g.png)    
   
## Service Discovery & DNS   
In container orchestration platforms things are a lot more complicated than traditional datacenters:   
- Hundreds or thousands of containers with individual IP addresses   
- Containers are deployed on varieties of different hosts, different data centers or even geolocations   
- Containers or Services need DNS to communicate. Using IP addresses is nearly impossible   
- Information about Containers must be removed from the system when they get deleted.   
   
The solution to the problem again is automation, all the information is put in a ***Service Registry***. **Finding other services in the network and requesting information about them is called *Service Discovery***.   
Approaches to Service Discovery:   
- **DNS** Modern DNS servers that have a service API can be used to register new services as they are created   
- **Key-Value-Store** Using a strongly consistent datastore especially to store information about services (**etcd**, **Consul**, **Apache Zookeeper**)   
   
   
## Service Mesh   
Monitoring, access control or encryption of the networking traffic is desired when containers communicate with each other.   
Instead of implementing all of this functionality into your application, you can just start a second container that has this functionality implemented.   
The software you can use to manage network traffic is called a **proxy**. This is a server application that sits between a client and server and can modify or filter network traffic before it reaches the server. Popular representatives are [nginx](https://www.nginx.com/), [haproxy](http://www.haproxy.org/) or [envoy](https://www.envoyproxy.io/).   
***A service mesh adds a proxy server to every container that you have in your architecture.***   
![image](..files\image_s.png)    
When a service mesh is used, applications don’t talk to each other directly, but the traffic is routed through the proxies instead. The most popular service meshes at the moment are [istio](https://istio.io/) and [linkerd](https://linkerd.io/).   
The proxies in a service mesh form the ***data plane***. This is where networking rules are implemented and shape the traffic flow.   
These rules are managed centrally in the ***control plane*** of the service mesh. This is where you define how traffic flows from service A to service B and what configuration should be applied to the proxies.   
So instead of writing code and installing libraries, you just write a config file where you tell the service mesh that service A and service B should always communicate encrypted.   
The config is then uploaded to the control plane and distributed to the data plane to enforce the new rule.   
   
## Storage   
From a storage perspective, containers have a significant flaw: they are ephemeral.   
Container images are read-only and consist of different layers that include everything that you added during the build phase. That ensures that every time you start a container from an image you get the same behavior and functionality.   
To allow writing files, **a read-write layer is put on top of the container image** when you start a container from an image.   
![image](..files\image_8.png)    
The problem here is that **this read-write layer is lost when the container is stopped or deleted**.   
If a container needs to persist data on a host, a ***volume*** can be used to achieve that: instead of isolating the whole filesystem of a process, directories that reside on the host are passed through into the container filesystem, ***this weakens the isolation of the container, giving access to the host filesystem***.   
Often, data needs to be accessed by multiple containers that are started on different host systems or when a container gets started on a different host it still should have access to its volume.   
Container orchestration systems like Kubernetes can help to mitigate these problems, but always require a robust storage system that is attached to the host servers.   
![image](..files\image_o.png)    
