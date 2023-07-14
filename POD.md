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

```shell
kubectl scale deploy/name--replicas=3 -n dev
```

```shell
kubectl get deploy/name -n dev
```

## Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  namespace: dev
  labels:
    owner: panda
    app: nginx
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
          image: nginx:latest
          ports:
            - containerPort: 80
```
