---
title: "What is Kubernetes?"
teaching: 10
exercises: 2
---

::::::::::::::::::::::::::::::::::::::: objectives

- Understand what Kubernetes is and when you might want to utilize it.
- Utilize Kubernetes commands to interact with a Kubernetes environment.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- What is Kubernetes and why might I want to use it?

::::::::::::::::::::::::::::::::::::::::::::::::::


Kubernetes, or "k8s" is an open source container orchestration platform to automate deploying, scaling, and managing containers within an environment. 
Traditionally, Kubernetes is aimed towards commercial environments and cloud infrastrutuce.
Originally developed by Google and realeased open source and is now managed by the Cloud Native Computing Foundation or CNCF. 

EXERCISE: Difficulties with software environments and moving across platforms. 

Advantages of K8s for research
✅- Common language for many platforms
-✅ NRP / Google / Amazon
-✅ Git / reproducible. 

Kubernetes provides a uniform platform and method across many different local and commericial cloud infrastructure. This ensures that your application is able to operate the same from your local developmment system to a production Kubernetes environment or a commercial cloud environment. This also helps ensure that a research workflow using Kubernetes is able to be reproduced on any infrastructure using Kubernetes.

Kubernetes deployments and environments are managed using [YAML](https://www.redhat.com/en/topics/automation/what-is-yaml) files. These YAML files allow you to tell the Kubernetes cluster what you want from a specific environment for the cluster then to attempt to make that defined configuration. This also increases the ability to change things in a recorded manner using version control tools like Git. 


EXERCISE: Basic commands for verifcation that the install works or they have access
`kubectl get nodes` / pods / deployments / services / namespaces


✅Commonly controlled with Yaml files and `kubectl apply -f FILENAME`

Explanation of API Server and expected state and observed state. 

The `kubectl` command does not directly control the state of a Kubernetes cluster. Instead it communicates directly to the Kubernetes' controller through an API server. When you run `kubectl apply` or `kubectl delete`, the command execution doesn't directly change the environment. Instead it tells the controller through the API server an expected state of the environment and the Kubernetes controller will attempt to make the declared state the current state and keep the cluster in the desired state. This is called "declarative management", which simplifies the management of more complex systems. 

-- Compare to managing many docker containers. 

Maybe split this from Intro -> Intro + CLI
- Explore kubectl help, kubectl CMD help
- This can be moved to the individual lessons to build an applied knowledge. 



