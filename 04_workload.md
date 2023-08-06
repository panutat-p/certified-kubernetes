# Workload Resources

https://kubernetes.io/docs/concepts/workloads/controllers

## Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  namespace: reddit
  labels:
    owner: panda
    app: nginx
spec:
  containers:
    - name: nginx
      image: nginx:1.25-bookworm
      ports:
        - containerPort: 80
```

## Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  namespace: reddit
spec:
  replicas: 3
  selector:
    matchLabels:
      owner: panda
      app: nginx
  template:
    metadata:
      labels:
        owner: panda
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:1.25-bookworm
          ports:
            - containerPort: 80
```

## Jobs

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: simple-job
spec:
  template:
    spec:
      containers:
      - name: busybox
        image: busybox:1.36
        command: ["sh", "-c"]
        args: ["echo 'Current time is: $(date)'"]
      restartPolicy: OnFailure
  backoffLimit: 5
```


## CronJob

Echo the current time every 10 seconds.

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: simeple-cron-job
spec:
  schedule: "*/10 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: busybox
            image: busybox:1.36
            command: ["sh", "-c"]
            args: ["echo 'Current time is: $(date)'"]
          restartPolicy: OnFailure
  successfulJobsHistoryLimit: 5
  failedJobsHistoryLimit: 5
```
