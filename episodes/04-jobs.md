---
title: "Jobs in Kubernetes"
teaching: 10
exercises: 2
---

::::::::::::::::::::::::::::::::::::::: objectives

- Understand what a Job is. 
- Explore the usage of Jobs to run computational workflows.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- What are Jobs and how can I use it for my workflows?

::::::::::::::::::::::::::::::::::::::::::::::::::

NRP Example: https://docs.nationalresearchplatform.org/userdocs/running/jobs

Kubernetes jobs are used to run tasks to completion, such as the end of a specific step in workflow or the complete end of a workflow. 
In the YAML file, jobs and pods appear very similar, but have a few extra pieces. This is a result of Jobs being a high-level abstration that manages a pod to make sure they run to completition, retry a set amount of times until they comlpete succesfully or run out of attempts. Jobs can also be set to run multiple pods of the same Pod at the same time. Jobs are also good for one-off or scheduled tasks such as data updates or backups and can be set to run on a schedule as a Cronjob. 

This makes pods very advantageous for computational scientific workflows where each step may only need to run once for each step, or can split out and run the sa,e part across multiple files. Jobs also enable easier monitoring of the current workflow stage since you only need to specify the Job rather than multiple pods. 

What are their benefits for research computing? 

