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

## Creating a PVC

We can go ahead and create a PVC to store some data. 

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

Data from a local computer can also then be copied to a PVC through a pod and using the mount path of the PVC. 
```bash
kubectl cp /path/to/file pod-name:/path/to/PVC
```

Exercise - Transfer data and view logs or something like that. 

