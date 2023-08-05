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
