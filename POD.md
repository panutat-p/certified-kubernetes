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

## Pod

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
```

## Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: dev
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        env: dev
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          ports:
            - containerPort: 80
```