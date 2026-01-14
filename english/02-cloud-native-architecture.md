# 02. Cloud Native Architecture   
## Cloud Native Architecture Fundamentals   
The CNCF defines the term ***cloud native*** as the following:   
"*Cloud native technologies empower organizations to build and run scalable applications in modern, dynamic environments such as public, private, and hybrid clouds. Containers, service meshes, microservices, immutable infrastructure, and declarative APIs exemplify this approach.*   
*These techniques enable loosely coupled systems that are resilient, manageable, and observable. Combined with robust automation, they allow engineers to make high-impact changes frequently and predictably with minimal toil.* *[…]*"   
**Monolithic vs Microservices Architecture**   
Monolithic Architecture: an application usually has a single code base and is provided as a single binary file that can run on a server   
- all the funcionalities are in the same application   
- easy to develop and deploy   
- very difficult to manage complexity, scale across multiple teams, implement changes fast or scale efficiently   
   
Microservices Architecture: multiple decoupled applications that communicate with each other in a network   
- the funcionalities are provided in many different applications   
- easy to manage complexity   
- easy to maintain applications   
- operate and scale applications individually   
- scale efficiently and across multiple teams   
- each team can hold ownership of different functions   
   
   
## Characteristics of Cloud Native Architecture   
Characteristics of Cloud Native Architecture are:   
- **High level of automation** automation in every step from development to deployment can be achieved by using modern automation tools and CI/CD pipelines that are backed by a Version Control System like git   
- **Self healing** cloud native application frameworks and infrastructure components include health checks which help monitor the application from inside and automatically restart if it fails (or if necessary)   
- **Scalable** scaling describes the process of handling more load while still providing a pleasant user experience (see Vertical/Horizontal scaling)   
- **Cost-Efficient** just like scaling up your application for high traffic situations, scaling down your application and the usage-based pricing models of cloud providers can save costs if traffic is low   
- **Easy to maintain** using *Microservices* allows to break down applications in smaller pieces and make them more portable, easier to test and to distribute across multiple teams   
- **Secure by default** cloud environments are often shared between multiple customers or teams, which calls for different security models; patterns like "zero trust computing" requires authentication from every user and process   
   
> reference twelve-factor app (methodology for building software-as-a-service) at 12factor.net   

> reference zero trust security model (approach to the strategy, design and implementation of IT systems) at  zero trust security model   

   
## Autoscaling   
The autoscaling pattern describes the dynamic adjustment of resources based on the current demand. CPU and memory are the obvious metrics to decide on when to scale applications as load increases or decreases, but other methods based on time or business metrics can also be considered to scale your services up or down.   
- **Vertical Scaling** describes the change in size of the underlying hardware, which only works within certain hardware limits for bare metal, but also for virtual machines   
- **Horizontal Scaling** describes the process of spawning new compute resources which can be new copies of your application process, virtual machines, or - in a less immediate way - even new racks of servers and other hardware   
   
***Talking about Autoscaling we are talking about Horizontal Scaling.***   
   
## Serverless   
**Cloud providers suggest that it is very easy to deploy applications, but require that you prepare and configure several resources like a network, virtual machines, operating systems and load balancers to run a simple web application. The idea of serverless computing is to relieve developers of these complicated tasks.**   
All popular cloud vendors have one or more commercial offerings of proprietary serverless runtimes and a subset of serverless, also known as Function as a Service (FaaS).   
Serverless computing has an even stronger focus on the on-demand provisioning and scaling of applications. Autoscaling is an important core concept of serverless.   
Although there are many advantages to serverless technology, it initially struggled with standardization. To address these problems, the CloudEvents project was founded and provides a specification of how event data should be structured. Events are the basis for scaling serverless workloads or triggering corresponding functions.   
## **Open Standards**   
Many cloud native technologies rely heavily on open source software, a common problem is how to build and distribute software packages, *containers* evolved as a standardized way to package and ship modern software.   
**Open Container Initiative (OCI) **provides three standards which define the way how to build and run containers:
   
- **image-spec** defines how to build and package container images   
- **runtime-spec** specifies the configuration, execution environment and lifecycle of containers   
- **distribution-spec** (more recent addition) provides a standard for the distribution of content in general and container images in particular   
   
Other important standards are:   
- **Container Network Interface (CNI)** a specification on how to implement networking for containers   
- **Container Runtime Interface (CRI)** a specification on how to implement container runtimes in container orchestration systems   
- **Container Storage Interface (CSI)** a specification on how to implement storage in container orchestration system   
- **Service Mesh Interface (SMI)** a specification on how to implement Service Meshes in container orchestration systems with a focus on Kubernetes   
   
   
## Cloud Native Roles & Site Reliability Engineering   
Jobs in cloud computing are more difficult to describe and the transitions are smoother, since the responsibilities are often shared between multiple people coming from different areas and with different skills   
Cloud Native Roles:   
- **Cloud Architect** responsible for adoption of cloud technologies, designing application landscape and infrastructure, with a focus on security, scalability and deployment mechanisms   
- **DevOps Engineer** use tools and processes that balance out software development and operations   
- **Security Engineer**   
- **DevSecOps Engineer** combines the previous two roles, often used to build bridges between more traditional development and security teams   
- **Data Engineer** collects, stores and analyzes the vast amounts of data that are being or can be collected in large systems   
- **Full-Stack Developer** all-rounder who is at home in frontend and backend development, as well as infrastructure details   
- **Site Reliability Engineer** (SRE) creates and maintains software that is reliable and scalable; to measure performance and reliability, SREs use three main metrics:   
    1. ***Service Level Objectives (SLO)*** "*Specify a target level for the reliability of your service*" - e.g. Reaching a latency of less than 100ms   
    2. ***Service Level Indicators (SLI)*** "*A carefully defined quantitative measure of some aspect of the level of service that is provided" - *e.g. How long a request actually needs to be answered   
    3. ***Service Level Agreements (SLA)*** "*An explicit or implicit contract with your users that includes consequences of meeting (or missing) the SLOs they contain. The consequences are most easily recognized when they are financial – a rebate or a penalty – but they can take other forms*"   
   
   
## Community and Governance   
The CNCF has a Technical Oversight Committee (TOC) that is responsible for defining and maintaining the technical vision, approving new projects, accepting feedback from the end-user committee, and defining common practices that should be implemented in CNCF projects.   
At the same time, the TOC does not control the projects, but encourages them to be self-governing and community owned and practices the principle of “minimal viable governance”.   
Since open source projects are dependent on their respective communities, governance is much different from traditional approaches. The great freedom of cloud native technologies forces people to impose rules on themselves and to enforce them.   
   
## CNCF Graduation Criteria v1.3   
There is a maturity level assigned to each CNCF initiative. Projects proposed to the CNCF should specify their preferred degree of maturity.   
A project must receive a two-thirds supermajority in order to be approved as incubating or graduated.   
Any graduated votes are considered as votes to join as an incubating project if there is not a supermajority of votes to do so.   
Any graduated or incubating votes are considered as sponsorship to enter as a sandbox project if there is not a supermajority of votes to enter as an incubating project.   
The project is turned down if there is not enough funding to qualify it for the sandbox stage.   
***Fallback voting*** is the term for this method of choosing.   
Project Maturity Levels:   
- **Sandbox Stage** this stage is the entry point for early stage projects   
- **Incubating Stage** the project to be accepted to the incubation stage must have met the sandbox stage requirements plus other technical due diligence has been performed such as: healthy number of committers, used in production by at least 3 adopters, clear versioning scheme, documented security processes, etc.   
- **Graduation Stage** to graduate a project must meet the incubation stage criteria plus other requirements such as: committers from at least 2 organizations, have completed an independent and third party security audit, have a public list of project adopters, receive supermajority vote from the TOC to move to graduation stage, etc.   
