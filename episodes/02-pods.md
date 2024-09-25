---
title: "Running Pods in Kubernetes"
teaching: 10
exercises: 2
---


::::::::::::::::::::::::::::::::::::::: objectives

- Understand what Kubernetes is and when you might want to utilize it.
- 

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- What is Kubernetes and why might I want to use it?

::::::::::::::::::::::::::::::::::::::::::::::::::

Pods are the fundamental building block of a Kubernetes setup and are designed to run a single portion of an application or process. 

Each pod has its own identifiers and specifications and containers. With Kubernetes, the individual container or containers in a pod are not managed by `kubectl`. Instead the pods are the lowest level of a Kubernetes object that is managed with `kubectl` for applications. 

Pods are designed around the idea of running a single portion of a larger system. Similar to each team in a company working on their own tasks. Sometimes these teams only have one person, or they may have multiple people. Pods have the ability to run with a single container or with multiple containers. If a pod is running multiple containers, they should be tightly coupled together. An example would be a pod running a simple website that is based on a Github repository. One container would be running the webserver itself and another would be pulling the repository updates. 

Pods running with mutliple containers are able to also share storage volumes, which allows each of the containers to work on the same location in a straight forward manner. 

## Where do container images come from?

Kuberenetes pulls container images from the same sources that Docker would. This can be from the local system, DockerHub, or another remote registry service. Any images that are not already available. The syntax is also the same as what you would use for Docker. 

:::::::::::::::::::::::::::::::::::::::::: spoiler

## Example formats

- `hello-world` - If you use the image name only, it will pull the latest tag from DockerHub
- `hello-world:1.2.3` - You can also specify a tag to select a specific version or build of an image. This should be specified when possible to help increase the reproducability of your workflows.
- `registry.example.com/hello-world:1.2.3` - This would pull a specific image and tag from a remmote registry. This could be a public registry or an insitutional registry.

::::::::::::::::::::::::::::::::::::::::::::::::::

## Exploring Pods

We are going to create a basic "hello world" pod. Below is a YAML file containing all of the information that is needed for the API controller to spawn our pod and its container. 

`hello-world.yaml`
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hello-world-pod
spec:
  containers:
  - name: hello-world-container
    image: busybox
    command: ["/bin/sh", "-c"]
    args: ["echo 'Hello World! I am in a pod!' && sleep infinity"]
```

### What do the lines mean?

When we apply or send the YAML file to the api Controller, the file's contents tell the controller what the desired state of your configuration is. 
The `apiVersion` line states what version of Kubernetes API the file is going to use. 
The `kind` line specifies what type of definition we are creating with this block of config
The `metadata` section contains basic information about the Pod we are creating. In this case we are only specifying the name of the pod. 
`spec` is the specifications of what we want the desired state of the pod to me. This file contains a basic single container Pod's specification.
The specification is asking for one container named `hello-world-container` using the `busybox` image.  
The `command` is what command we want the container to run, similar to changing the entrypoint using `docker run` and the args are the commands we will be running inside the `hello-world-container` container within our `hello-world` Pod.

We can send this configuration to the API Controller using the `kubectl apply` command. 
```bash
kubectl apply -f hello-world.yaml
```
```output
pod/hello-world-pod created
```

We can check the status of our Pod using `kubectl get`
```bash
kubectl get pods
```
```output
NAME              READY   STATUS              RESTARTS   AGE
hello-world-pod   0/1     ContainerCreating   0          9s
```
The output indicates that our single pod is not ready for use and is currently creating all of the specified containers. Since we are only using one container, this process should be fairly quick. 

If we check again shortly after, we can see the Pod is ready and running. 
```output
NAME              READY   STATUS    RESTARTS   AGE
hello-world-pod   1/1     Running   0          5s
```

::: callout
We can get more details about the Pod including the creation process and progress today using `kubectl describe name-of-pod`


:::

We can check if our Pod has run or is running by checking the logs using `kubectl logs name-of-pod`.
```bash
kubectl logs hello-world-pod
```
```output
Hello World! I am in a pod!
```

EXERCISE -- Basic Hello World Pod
Explanation with it

-- Describe Pod Demo

-- Pod States

-- Delete the pod

EXERCISE -- Modify the YAML and try it out. What is the state, what did you change. 

-- Check the logs



What are pods, Hello world, CLI runs, Yaml Runs, where do container images come from
Other basic configurations

