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

## Jobs

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: simple-job
  namespace: demo
spec:
  template:
    metadata:
      labels:
        owner: admin
        app: simple-job
    spec:
      serviceAccountName: default
      containers:
      - name: busybox
        image: busybox:1.36
        command: ["sh", "-c"]
        args: ["echo 'Current time is: $(date)'"]
      restartPolicy: OnFailure
  backoffLimit: 5
```

## CronJob

* Run every 5 seconds
* Each job has deadline of 8 seconds

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: every-5s-cron
  namespace: demo
  labels:
    app: every-5s-cron
    owner: admin
spec:
  schedule: "*/5 * * * *"
  successfulJobsHistoryLimit: 5
  failedJobsHistoryLimit: 10
  jobTemplate:
    spec:
      activeDeadlineSeconds: 5
      template:
        metadata:
          labels:
            app: every-5s-job
            owner: admin
        spec:
          serviceAccountName: default
          containers:
            - name: busybox
              image: busybox:1.36
              command:
                - sh
                - -c
                - wget -q -O /dev/stdout --auth-no-challenge https://cat-fact.herokuapp.com/facts
          restartPolicy: Never
      backoffLimit: 0
```
