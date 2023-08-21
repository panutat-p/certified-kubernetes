# Workload Resources

https://kubernetes.io/docs/concepts/workloads/controllers

## Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: ubuntu
  namespace: demo
  labels:
    owner: admin
    app: ubuntu
spec:
  serviceAccountName: default
  securityContext:
    runAsUser: 1000
    runAsGroup: 3000
    fsGroup: 2000
  containers:
    - name: ubuntu
      image: ubuntu:23.10
      ports:
        - containerPort: 80
      command: ['sleep']
      args: ['infinity']
      securityContext:
        capabilities:
          add: ['NET_ADMIN', 'SYS_TIME']
```

## Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  namespace: demo
spec:
  replicas: 3
  selector:
    matchLabels:
      owner: admin
      app: nginx
  template:
    metadata:
      labels:
        owner: panda
        app: nginx
    spec:
      serviceAccountName: default
      containers:
        - name: nginx
          image: nginx:1.25-bookworm
          ports:
            - containerPort: 80
```

```shell
kubectl rollout restart deploy/name    
```

```shell
kubectl rollout status deploy/name
```

```shell
kubectl rollout undo deploy/name
```

## Job

https://kubernetes.io/docs/concepts/workloads/controllers/job

* A container in a pod may fail: non-zero exit code or memory limit exceeded
  * `restartPolicy: Never` terminate the pod
  * `restartPolicy: OnFailure` the pod stays on the node but the container is re-run
* 💀 An entire Pod may fail: the node is upgraded or rebooted
* `completions` (default is 1)
  * The second pod will be created after the first pod is successful because `parallelism` is 1
  * The job is considered as `Succeeded` when there are 3 successful pods
* `backoffLimit` (default is 6)
  * The job is considered as `Failed` when the number of retries exceeds the `backoffLimit`

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: random-error-job
  namespace: demo
spec:
  completionMode: NonIndexed
  completions: 3
  parallelism: 1
  backoffLimit: 6
  template:
    metadata:
      labels:
        owner: admin
        app: random-error-job
    spec:
      serviceAccountName: default
      containers:
      - name: random-error
        image: kodekloud/random-error
      restartPolicy: Never
```

## CronJob

* Run every 5 minutes
* Each job has deadline of 15 seconds
* Keep 5 successful job instances
* Keep 10 failed job instances

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: cat-cron
  namespace: demo
  labels:
    app: cat-cron
    owner: admin
spec:
  schedule: "*/5 * * * *"
  successfulJobsHistoryLimit: 5
  failedJobsHistoryLimit: 10
  jobTemplate:
    spec:
      activeDeadlineSeconds: 15
      template:
        metadata:
          labels:
            app: cat-job
            owner: admin
        spec:
          serviceAccountName: default
          containers:
            - name: curl
              image: curlimages/curl:latest
              command:
                - sh
                - -c
                - curl https://cat-fact.herokuapp.com/facts
          restartPolicy: Never
      backoffLimit: 0
```

```shell
kubectl create job cat-test-job --from cronjob/cat-cron
```

Expected: see at most 5 successful jobs and at most 10 failed jobs
```shell
kubectl get job
```
