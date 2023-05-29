# Config Maps

## Redis example

`configmap.yaml`

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: redis-config
data:
  REDIS_BIND_ADDRESS: "0.0.0.0"
  REDIS_PORT: "6379"
  REDIS_TIMEOUT: "300"
  REDIS_PASSWORD: "mypassword"
```

`redis.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis-app
  template:
    metadata:
      labels:
        app: redis-app
    spec:
      containers:
        - name: redis-container
          image: redis
          ports:
            - containerPort: 6379
          envFrom:
            - configMapRef:
                name: redis-config
```

```shell
kubectl apply -f configmap.yaml -f redis.yaml
```

## Complex configurations

https://stackoverflow.com/a/66455621
