---
title: "Deployments in Kubernetes"
teaching: 10
exercises: 2
---

::::::::::::::::::::::::::::::::::::::: objectives

- Understand what Deployments do at the control plane # This needs to be better
- Create and deploy Deployments to a Kubernetes cluster. 

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- What are deployments and why might I want to use it?

::::::::::::::::::::::::::::::::::::::::::::::::::

::: callout
## How would you coordinate multiple pods?
For workflows or environments with a pod or two, managing the pods is a trivial task. What about 5, 10, or 20 pods working together? 
:::

Deployments are a way to manage the spawning and manipulation of many pods working together on a larger set of applications. 
As we use more pods for different steps or iterations, we may get to a point where multi pods will need to work together. This can be something small like a pod running a database in conjunction with another pod doing operations within the database. Another case, would be deployment packages that have multiple components working together in tandem or they may be part of a larger infrastructure like a science gateway. 

More information on science gateways is available at https://sciencegateways.org/. 

## Creating and running a deployment

Similar to creating Pods, Volumes, and Jobs, deployments have the same general structure of metadata and definitions. 

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-first-deployment
spec:
  ...
```

DESCRIBE the different parts of SPEC. 

We can go ahead and build our first Deployment yaml file add the `spec` of the deployment.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-first-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      k8s-app: test-dep
  template:
    metadata:
      labels:
        k8s-app: test-dep
    spec:
      containers:
      - name: mypod
        image: ubuntu
        resources:
           limits:
             memory: 500Mi
             cpu: 500m
           requests:
             memory: 100Mi
             cpu: 50m
        command: ["sh", "-c", "sleep infinity"]
```

SPECIFIC EXAMPLES 
- Where rolling updates, replicas, rollback, or monitoring may be better. 


# Notes:
- Talk about deployments. 
- Examples they may work well for research
- Deploy a deployment of some kind. 
- Manages replica sets and updates and has many features. 
- Benefit in R/SCI Computing?