# CKAD - Pod Design  

**Domain**	| **Weight** |
:------- | :-------------|
**Pod Design** | 20%
[3.1 Understand Deployments and how to perform rolling updates](CKAD-3-Pod-Design.md#31-Understand-deployments-and-how-to-perform-rolling-update-and-rollbacks) |  
[3.2 Understand Deployments and how to perform rollbacks](CKAD-3-Pod-Design.md#32-understand-deployments-and-how-to-perform-rollbacks) |  
[3.3 Understand Jobs and CronJobs](CKAD-3-Pod-Design.md#33-understand-jobs-and-cronjobs) |  
[3.4 Understand how to use Labels, Selectors, and Annotations](CKAD-3-Pod-Design.md#33-understand-jobs-and-cronjobs) |


## 3.1 Understand deployments and how to perform rolling update and rollbacks  
https://kubernetes.io/docs/tutorials/kubernetes-basics/update/update-intro/

### Deployments  
https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
* Scaling up / down easily number of replicas
* Rolling updates to deploy new SW version  

<br/>

Here's an example of a deployment for an NGINX app:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-deployment
  template:
    metadata:
      labels:
        app: my-deployment
    spec:
      containers:
      - name: nginx
        image: nginx:1.19.1
        ports:
        - containerPort: 80
```
<br/>

### Rolling Updates  
https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#updating-a-deployment

Three options for rolling updates:

1- Using imperative command `kubectl set image`:
(use `--record` to easily rollback)
```
kubectl set image deployment/my-deployment nginx=nginx:1.19.2 --record
```

2- Using imperative command `kubectl edit deploy`:
```
kubectl edit deployment my-deployment
```
Note: any change is applied immediately, no need for `kubectl apply`

3- Editing specs in the deployment YAML manifest:
```
vi deployment.yaml
kubectl apply -f deployment.yaml
```

Check deployment status:
```
kubectl rollout status deployment my-deployment
kubectl rollout status deployment/my-deployment
kubectl rollout history deployment my-deployment
```


## 3.2 Understand deployments and how to perform rollbacks  
https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#rolling-back-a-deployment

Three options for rolling back:

1- Using imperative command `kubectl rollout undo`:
```
kubectl rollout history deployment/my-deployment
kubectl rollout undo deployment.v1.apps/my-deployment
kubectl rollout undo deployment.v1.apps/my-deployment --to-revision=1
```
Note: use `--to-revision` when `—-record` was used


2- Using imperative command `kubectl edit deploy`
```
kubectl edit deployment my-deployment
kubectl rollout status deployment my-deployment
```
Note: any change is applied immediately, no need for `kubectl apply`


3- Editing specs in the deployment YAML manifest:
```
vi deployment.yaml
kubectl apply -f deployment.yaml
```

<br/>

Check rollout status:
```
kubectl rollout status deployment my-deployment
kubectl rollout history deployment my-deployment
```
<br/>


## 3.3 Understand Jobs and CronJobs

### Jobs  
https://kubernetes.io/docs/concepts/workloads/controllers/job/
* A Job create 1 or more Pods to run tasks to completion
* Jobs track successful completion of tasks

<br/>

Here's an example of a Job to clean up the content of /data directory on worker nodes:
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: cleanup
spec:
  template:
    spec:
      containers:
      - name: cleanup
        image: busybox
        command: ["/bin/sh",  "-c", "rm -rf /data/*"]
        volumeMounts:
        - mountPath: /data
          name: data
      restartPolicy: Never
  backoffLimit: 4
  volumes:
  - name: data
    hostPath:
      path: /data
```
<br/>

### Cronjobs  
https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/
* CronJob createa Jobs on a repeating schedule.
* runs a job periodically on a given schedule, written in Cron format.

**Cron schedule syntax**
```
# ┌───────────── minute (0 - 59)
# │ ┌───────────── hour (0 - 23)
# │ │ ┌───────────── day of the month (1 - 31)
# │ │ │ ┌───────────── month (1 - 12)
# │ │ │ │ ┌───────────── day of the week (0 - 6) (Sunday to Saturday;
# │ │ │ │ │                                   7 is also Sunday on some systems)
# │ │ │ │ │
# │ │ │ │ │
# * * * * *
```
Entry	| Description	| Equivalent to
:-------| :-------------|:-------------
@yearly (or @annually) | Run once a year at midnight of 1 January	| 0 0 1 1 *  
@monthly | Run once a month at midnight of the first day of the month | 0 0 1 * *  
@weekly | Run once a week at midnight on Sunday morning | 0 0 * * 0  
@daily (or @midnight) | Run once a day at midnight | 0 0 * * *
@hourly | Run once an hour at the beginning of the hour | 0 * * * *

<br/>

Sample Cronjob:  

```yaml
apiVersion: batch/v1beta1
kind: CronJob
metadata:
  name: hello
spec:
  schedule: "*/1 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: hello
            image: busybox
            imagePullPolicy: IfNotPresent
            command:
            - /bin/sh
            - -c
            - date; echo Hello from the Kubernetes cluster
          restartPolicy: OnFailure
```
<br/>


## 3.4 Understand how to use Labels, Selectors, and Annotations

### Labels
https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/

Here is a pod with some labels.
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-with-label
  labels:
    app: my-app
    environment: production
spec:
  containers:
  - name: nginx
    image: nginx
```

You can view existing labels with `kubectl describe`.

Here is another pod with different labels.
```
kubectl describe pod my-production-label-pod
```

You can use various selectors to select different subsets of objects.
```
kubectl get pods -l app=my-app
kubectl get pods -l environment=production
kubectl get pods -l environment=development
kubectl get pods -l environment!=production
kubectl get pods -l 'environment in (development,production)'
kubectl get pods -l app=my-app,environment=production
```
<br/>

### Annotations
https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/

Here is a simple pod with some annotations.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-with-annotation
annotations:
  owner: joe@example.com
  git-commit: bdab0c6 
spec:
  containers:
  - name: nginx
  image: nginx
```

Like labels, existing annotations can also be viewed using 
`kubectl describe`.

```
kubectl describe pod my-annotation-pod
```