# 06. Cloud Native Application Delivery   
## Application Delivery Fundamentals   
The best way to manage source code is a version control system.   
In 2005 Linus Torvalds created *Git*, a decentralized system that can be used to track changes in your source code.   
With your source code in check, the next step before delivering your application is to build it.   
To ensure high quality of your application, the next step should be extensive and automatic testing of the app to make sure all functionality is still in place after someone makes changes.   
The last step is delivering the application to the platform it should run on.   
   
## CI/CD   
***Continuous Integration*** is the first part of the process and ***describes the permanent building and testing of the written code***. High automation and usage of version control allows multiple developers and teams to work on the same code base.   
***Continuous Delivery*** is the second part of the process and ***automates the deployment of the pre-built software***. In cloud environments, you will often see that software is deployed to Development or Staging environments, before it gets released and delivered to a production system.   
To automate the whole workflow, you can use a **CI/CD pipeline**, which is actually nothing more than the scripted form of all the steps involved, running on a server or even in a container.   
Popular CI/CD tools include:   
- [Spinnaker](https://spinnaker.io/)   
- [GitLab](https://gitlab.com/)   
- [Jenkins](https://www.jenkins.io/)   
- [Jenkins X](https://jenkins-x.io/)   
- [Tekton CD](https://github.com/tektoncd/pipeline)   
- [ArgoCD](https://argoproj.github.io/)   
   
   
## GitOps   
Infrastructure as Code was a real revolution in increasing the quality and speed of providing infrastructure, and it works so well that today, configuration, network, policies, or security can be described as code, and often even live in the same repository as the software.   
There are two different approaches how a CI/CD pipeline can implement the changes you want to make:   
- **Push-based**
The pipeline is started and runs tools that make the changes in the platform. Changes can be triggered by a commit or merge request.   
- **Pull-based**
An agent watches the git repository for changes and compares the definition in the repository with the actual running state. If changes are detected, the agent applies the changes to the infrastructure.   
   
Two examples of popular GitOps frameworks that use the pull-based approach are [Flux](https://fluxcd.io/) and [ArgoCD](https://argo-cd.readthedocs.io/).   
ArgoCD is implemented as a Kubernetes controller, while Flux is built with the GitOps Toolkit, a set of APIs and controllers that can be used to extend Flux, or even build a custom delivery platform.   
***Kubernetes is using a similar idea as the pull based approach: A database is watched for changes and the changes are applied to the running state if it doesn’t match the desired state.***   
   
