# Config Maps

## ConfigMap as ENV

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
---
apiVersion: v1
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

## ConfigMap as volume

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
    - name: nginx-container
      image: nginx:1.25-bookworm
      ports:
        - containerPort: 80
      volumeMounts:
        - name: nginx-config
          mountPath: /etc/nginx/nginx.conf
          subPath: nginx.conf
  volumes:
    - name: nginx-config
      configMap:
        name: nginx-configmap

```

## Complex configurations

https://stackoverflow.com/a/66455621
