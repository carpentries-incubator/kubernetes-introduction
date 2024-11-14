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

::::::::::::::::::::::::::::::::::::::::::::::::::

Services are a way to expose a deployment or pod to the network outside of a kubernetes cluster for a long term. 

Short term example: https://docs.nationalresearchplatform.org/userdocs/running/monitoring/#tensorboard

When to use a service

Run a basic web service with a web result. 

# Notes from MU Lesson
- Each pod has a unique IP and it changes everytime a pod is restarted
- Services enable COM betweeen apps within Cluster and external if needed.
- Types:
    - Cluster IP
    - Node Port
    - Load Balance
    - Ingress