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


Kubernetes, or "k8s" is a container orchestration platform to manage and deploy containers to an environment. Traditionally,
Kubernetes is aimed towards enterprise environments and hosting infrastrutuce.
Originally developed by Google and realeased open source ....

Difficulties with software environments and moving across platforms. 

Advantages of K8s for research
- Common language for many platforms
- NRP / Google / Amazon
- Git / reproducible. 

Basic commands for verifcation that the install works or they have access
`kubectl get nodes` / pods / deployments / services / namespaces


Commonly controlled with Yaml files and `kubectl apply -f FILENAME`

Explanation of API Server and expected state and observed state. 

Maybe split this from Intro -> Intro + CLI
- Explore kubectl help, kubectl CMD help



