# Kubernetes Definition Files

```shell
kubectl create -f file.yaml
```

```shell
kubectl describe deployment/name
```

```shell
kubectl apply -f ./file.yaml
```

```shell
kubectl create deployment d-sample --image=nginx --dry-run=client -o yaml > d-sample.yaml
```

```shell
kubectl create pod p-sample --image=nginx --port=80 --dry-run=client -o yaml > p-sample.yaml
```

## Pod definition file

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  namespace: dev
spec:
  containers:
    - name: nginx
      image: nginx:latest
      ports:
        - containerPort: 80
      env:
        - name: ENV
          value: dev
```

## Deployment definition file

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-7o672f
spec:
  replicas: 3
  selector:
    matchLabels:
      app: backend
  template:
    metadata:
      labels:
        app: backend
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
        env:
        - name: APP
          value: sample-nginx
        - name: ENV
          value: dev
```
