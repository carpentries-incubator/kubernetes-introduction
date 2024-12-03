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
The `kind` line specifies what type of object we are creating with this block of config
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

## Getting more details

We can get more details about the Pod including the creation process and progress today using `kubectl describe pod name-of-pod`

The `kubectl describe` provides a verbose description of different Kubernetes resources. You do need to specify both the type or `kind` of object and the name of the object itself that you want information on. 

:::::::::::::::::::::::::::::::::::::::::: spoiler

```bash
kubectl describe pod hello-world-pod
```

```output
...
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  18m   default-scheduler  Successfully assigned default/hello-world-pod to minikube
  Normal  Pulling    18m   kubelet            Pulling image "busybox"
  Normal  Pulled     18m   kubelet            Successfully pulled image "busybox" in 1.638s (1.638s including waiting). Image size: 4261502 bytes.
  Normal  Created    18m   kubelet            Created container hello-world-container
  Normal  Started    18m   kubelet            Started container hello-world-container
```

::::::::::::::::::::::::::::::::::::::::::::::::::

:::

We can check if our Pod has run or is running by checking the logs using `kubectl logs name-of-pod`.
```bash
kubectl logs hello-world-pod
```
```output
Hello World! I am in a pod!
```

## Running commands in Pods

During development and debugging of environments or verifying a quick command, it may be beneficial to get an interactive terminal within a pod or run a command within the pod. 

We can pull up an interactive terminal in a pod using kubectl's `exec` command. 

```bash
kubectl exec -it hello-world-pod -- /bin/sh
```

```output
/ #
```

To get out of the interactive session, run the `exit` command. 

::::::::::::::::::::::::::::::::::::: challenge

## Explore the pod

Run some commands within the pod to explore its environment. Some questions that can be answered are: 

- What is the hostname of the Pod? 
- What user are we running as within the Pod?

::::::::::::::::::::::::::::::::::::::::::::::::::

Individual commands can also be executed from within the Pod without the need for an interactive session. 
```bash
kubectl exec -it hello-world-pod -- /bin/sh -c "hostname"
```

```output
hello-world-pod
```

## Cleaning up

We can clean up our pods from this lesson by deleting them using kubectl's `delete` command. 
To do this, we can either run the `delete` command similar to the `apply` command by specifying the file, or we can manually delete it by calling its name. 

By specifying the filename, anything that is defined in the file will get removed. 
```bash
kubectl delete -f hello-world.yaml
```

```output
pod "hello-world-pod" deleted
```

We can also run the same operation by specifying the resource type and resource name of the pod we are deleting. 

```bash
kubectl delete pod hello-world-pod
```

```output
pod "hello-world-pod" deleted
```

When we use the file itself to delete the Pod, the definitions provided in the file automatically handle the resource name and type, while without the file, that information is needed in order for the Kubernetes controller to make an operation. 



## Modifying the Pod

While a pod is running, we can modify it's definitions and the API controllers will handle the transition for us. 

If we switch the container definition in the Pod yaml file from `busybox` to another container, such as `ubuntu:23.04`, we can migrate our Pod. 

After making the change, we can re-apply the definition file. 

```bash
kubectl apply -f hello-world.yaml
```

```output
pod/hello-world-pod configured
```

The language has changed when we applied the new definition file to our Pod. When we made our Pod for the first time, it listed the Pod as "created". In this instance the Pod was "configured" and not "created". 

We can also look at the actions that the Controller took to update our pod using the `describe` command. 

```bash
kubectl describe pod hello-world-pod
```

```output
...
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  40s   default-scheduler  Successfully assigned default/hello-world-pod to minikube
  Normal  Pulling    39s   kubelet            Pulling image "busybox"
  Normal  Pulled     38s   kubelet            Successfully pulled image "busybox" in 718ms (718ms including waiting). Image size: 4269694 bytes.
  Normal  Created    37s   kubelet            Created container hello-world-container
  Normal  Started    37s   kubelet            Started container hello-world-container
  Normal  Killing    18s   kubelet            Container hello-world-container definition changed, will be restarted
  Normal  Pulling    4m53s                  kubelet            Pulling image "ubuntu:23.04"
  Normal  Pulled     4m50s                  kubelet            Successfully pulled image "ubuntu:23.04" in 3.41s (3.41s including waiting). Image size: 70323206 bytes.
  Normal  Created    4m45s (x2 over 5m43s)  kubelet            Created container hello-world-container
  Normal  Started    4m45s (x2 over 5m43s)  kubelet            Started container hello-world-container
```

Here we can see that the Controller noticed the definition change, then worked to kill the existing Pod to remake it with the new definition. 

We can also verify that the container has changed by checking its operating system with `cat /etc/os-release`

```bash
kubectl exec -it hello-world-pod -- /bin/sh -c "cat /etc/os-release"
```

```output
PRETTY_NAME="Ubuntu 23.04"
NAME="Ubuntu"
VERSION_ID="23.04"
...
```

We could achieve a similar result by deleting the Pod, making the changes, then reapplying the definition, however this has us doing the effort of managing our Pods, when instead we can let Kubernetes handle it. 


_____

::: callout
## Clean up after the lesson

Go ahead and check for any remaining Pods and kill them before moving on.

:::

____



