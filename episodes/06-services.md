---
title: "Volumes in Kubernetes"
teaching: 10
exercises: 2
---

::::::::::::::::::::::::::::::::::::::: objectives

- Understand what Services in Kubernetes are.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- What are Kubernetes Services?
- What can Kubernetes Services be used for?

::::::::::::::::::::::::::::::::::::::::::::::::::

Services are a way to expose a deployment or pod to the network outside of a kubernetes cluster for a long term. 
When we create a Pod, it gets assigned a unique IP address that allows it to communicate with other pods. However, when a pod is restarted or deleted and recreated, it's IP address will change, which makes communication amongst pods more complicated. 

Services allow Pods to have more reliable communication by abstracting the networking layer between Pods and other Pods or the outside world. Examples of this could be having a internal database communicating with an application in another Pod doing analysis on the stored data. It could also be used to expose a science gateway to the public internet or a local campus network. 



