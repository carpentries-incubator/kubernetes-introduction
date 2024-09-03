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

Ask about cordination of spawning multiple pods 

Sometimes, a workflow may have multiple components working together in tandem or they may be part of a larger infrastructure like a science gateway. ((DEFINE SCIENCE GATEWAY WITH LINK)). For workflows or environments with a pod or two, managing the pods is a trivial task. What about 5, 10, or 20 pods working together? This is where deployments come in. 

Deployments are a way to coordinate multiple pods working together for more complex workflows or science gateways. 

SPECIFIC EXAMPLES 
- Where rolling updates, replicas, rollback, or monitoring may be better. 

Talk about deployments. 

Examples they may work well for research

Deploy a deployment of some kind. 