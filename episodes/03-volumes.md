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

Where is data stored in a pod? 

By default, Pods are stateless entities and should be treated as such. Anything the pod creates in it's filesystem is removed when the pod stops or dies. For temporary files and directories this is fine. However this is not condusive for research since results need to be saved somewhere. 

Stateless vs stateful

What are volumes?

How do I move data into the volumes?

Exercise - Transfer data and view logs or something like that. 

