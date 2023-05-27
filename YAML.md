# Output

`deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-server
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```

```shell
kubectl create -f ./deployment.yaml
```

```shell
kubectl apply -f ./deployment.yaml
```

```shell
kubectl describe deployment/web-server
```
