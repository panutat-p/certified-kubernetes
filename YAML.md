# Output

`deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-server
  labels:
    app: backend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: backend
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
        env:
        - name: APP
          value: sample-nginx
        - name: ENV
          value: dev
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
