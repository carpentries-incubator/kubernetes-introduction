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


## What are Jobs? 

Kubernetes Jobs are used to run tasks to completion, such as the end of a specific step in workflow or the complete end of a workflow. 
In the YAML file, Jobs and Pods appear very similar, but have a few extra pieces. This is a result of Jobs being a high-level abstration that manages a Pod to make sure they run to completition, retry a set amount of times until they comlpete succesfully or run out of attempts. Jobs can also be set to run multiple Pods of the same Pod at the same time. Jobs are also good for one-off or scheduled tasks such as data updates or backups and can be set to run on a schedule as a Cronjob. 

This makes pods very advantageous for computational scientific workflows where each step may only need to run once for each step, or can split out and run the same part across multiple files. Jobs also enable easier monitoring of the current workflow stage since you only need to specify the Job rather than multiple pods. 

The file structure for a Job is fairly similar to a Pod's yaml file. 

`pi-example.yaml`
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: pi
spec:
  template:
    spec:
      containers:
      - name: pi
        image: perl:5.34.0
        command: ["perl",  "-Mbignum=bpi", "-wle", "print bpi(2000)"]
      restartPolicy: Never
  backoffLimit: 4
```
Example from https://kubernetes.io/docs/concepts/workloads/controllers/job/


The example above will run a perl container that compute Pi to 2000 digits. 
The backoff Limit also prevents a Job from running continously in a crash loop if any of the Pods spawned by the Job fail. In the example above, if more than 4 Pods fail, it will cancel the Job. 

The example may take up to 2 minutes to complete. 

```bash
kubectl logs jobs/pi
```
```output
job.batch/pi created
```

After 2 minutes, check the logs.
```bash
kubectl logs jobs/pi
```
```output
3.141592653589793238462643383279502...
```


## What are their benefits for research computing? 



## Need to add or maybe add:
- Multi Pod
- Benefits for research computing
- More Examples
