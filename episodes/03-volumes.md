---
title: "Volumes in Kubernetes"
teaching: 10
exercises: 2
---

::::::::::::::::::::::::::::::::::::::: objectives

- Explore creating and using volumes
- Understand the "stateless" nature of pods

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How do I make data available in a Pod?

::::::::::::::::::::::::::::::::::::::::::::::::::

Pods are stateless entities and should be treated as such. Anything the pod creates in it's filesystem is removed when the pod stops or dies. For temporary files and directories this is fine. However this is not condusive for research since results need to be saved somewhere.  Pods and their containers are in a ephemeral or "stateless" configuration by default. This means that any data stored on the filesystems within the containers are lost when the pod is stopped. For workflows, this creates a small barrier. Workflows can still be run using this methodology, however the pods and their containers would need to be setup to download their input and upload their output to a remote server. 

Volumes are the solution to this problem. The main type of volume used for pods is the Persistent Volume Claim or "PVC". These are requests to the Kubernetes cluster to "claim" space on the storage system or a Persistent Volume (PV) of a cluster. PVCs are scoped to different namespaces, which means that a PVC is not visible or accessible by another namespace on the Kubernetes cluster. 

Once a PVC is mounted in a pod, data can be stored or retrieved from using the mount path on the container's filesystem. This allows data to stay persistent after the pod is terminated. 

## Creating a PV

:::::::::::::::::::::::::::::::::::::::::: spoiler

## Some setups may do this automatically

A Kubernetes cluster may automatically create a PV when a PVC is created. This will be mentioned in any documentation of the cluster itself. 


::::::::::::::::::::::::::::::::::::::::::::::::::

First we need to create a Persistent Volume to give us a space to claim for files. 

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv0001
spec:
  accessModes:
    - ReadWriteOnce
  capacity:
    storage: 5Gi
  hostPath:
    path: /mnt/pv0001/
```

- Details on PV 
- naming
- accessMode
- Storage
- Path

This will create a dedicated space for our Pods to store files. The Pods won't have immediate access to store data in a PV. In order for the Pods to store data, they need to use a claim against the PV using a Persistent Volume Claim.  

## Creating a PVC

For our Pods to store data, they need to claim space in a Persistent Volume. 

`pvc_storage.yaml`
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-test-pv-claim
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 3Gi
```

This configuration will look for a Persistent Volume to claim against if there is a mathcing and available PV. 

If there is an available and matching PV, a claim will then be available for a Pod or multiple Pods to use. 


```bash
kubectl apply -f pvc_storage.yaml
```
```output
persistentvolumeclaim/my-test-pv-claim created
```

This created a PVC and dynamically made a PV since a PV did not yet exist. 

We can check both by using `kubectl get pv` and `kubectl get pvc`. 

```bash
kubectl get pv
```
```output
NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                                          STORAGECLASS   VOLUMEATTRIBUTESCLASS   REASON   AGE
pvc-7c06f9ef-909a-4cd7-b450-d136219a8964   8Gi        RWO            Delete           Bound    openproject/data-my-openproject-postgresql-0   standard       <unset>                          83d
```


```bash
kubectl get pvc
```
```output
NAME               STATUS    VOLUME   CAPACITY   ACCESS MODES   STORAGECLASS   VOLUMEATTRIBUTESCLASS   AGE
my-test-pv-claim   Pending                                      manual         <unset>                 82s
```

## Transfering data to and from a PVC

In order to transfer data to and from a PVC, you need to have the volume mounted in a pod for the transfer. Data from a local computer can then be copied to a PVC through the Pod using the mount path of the PVC. 

First we will need to make a pod to copy our data through. We will use a similar structure as our first pod. 

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
    volumeMounts:
    - mountPath: /mnt/my_pvc
      name: my-pvc-for-pod
  volumes:
    - name: my-pvc-for-pod
      persistentVolumeClaim:
        claimName: my-test-pv-claim
```



```bash
kubectl cp /path/to/file pod-name:/path/to/PVC
```

Exercise - Transfer data and view logs or something like that. 

