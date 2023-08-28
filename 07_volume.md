# Storages

https://kubernetes.io/docs/concepts/storage

## Empty directory

* An `emptyDir` volume is first created (initially empty) when a Pod is assigned to a node
* An `emptyDir` volume is non-persistent and will lose its data if the pod is terminated or rescheduled to a different node
* A container crashing does not remove a Pod from a node, the data in an `emptyDir` volume is safe across container crashes
* An `emptyDir` volume shares storage between containers in the same Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
    - name: nginx
      image: nginx
      ports:
        - containerPort: 80
      volumeMounts:
        - name: html-volume
          mountPath: /usr/share/nginx/html
  volumes:
    - name: html-volume
      emptyDir:
        sizeLimit: "1Gi"
```

```shell
kubectl cp ~/index.html nginx-pod:/usr/share/nginx/html/index.html
```

## Host path

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: event-simulator
  namespace: demo
  labels:
    app: event-simulator
spec:
  containers:
    - name: event-simulator
      image: kodekloud/event-simulator
      volumeMounts:
        - name: log-volume
          mountPath: /log
  volumes:
    - name: log-volume
      hostPath:
        type: Directory
        path: /var/log/webapp

```

## ConfigMap as ENV

* A `ConfigMap` is always mounted as `readOnly`

https://stackoverflow.com/a/66455621

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
kind: ConfigMap
metadata:
  name: nginx-configmap
data:
  nginx.conf: |
    server {
      listen 8080;
      server_name localhost;
    }
---
apiVersion: v1
kind: Pod
metadata:
  name: website
  namespace: reddit
  labels:
    owner: panda
    app: website
spec:
  containers:
    - name: nginx
      image: nginx:1.25-bookworm
      ports:
        - containerPort: 80
      volumeMounts:
        - name: nginx-volume
          mountPath: /etc/nginx/nginx.conf
          subPath: nginx.conf
  volumes:
    - name: nginx-volume
      configMap:
        name: nginx-configmap
```

## Secret key as ENV

https://opensource.com/article/19/6/introduction-kubernetes-secrets-and-configmaps

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: redis-secret
type: Opaque
data:
  redis-password: base64_encoded_password
---
apiVersion: v1
kind: Pod
metadata:
  name: redis
spec:
  containers:
    - name: redis
      image: redis:7
      env:
        - name: REDIS_PASSWORD # container variable name
          valueFrom:
            secretKeyRef:
              name: redis-secret
              key: redis-password # key in secret
```

## Secret as ENV

https://kubernetes.io/docs/tasks/inject-data-application/distribute-credentials-secure/#configure-all-key-value-pairs-in-a-secret-as-container-environment-variables

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: redis-secret
type: Opaque
data:
  REDIS_PASSWORD: base64_encoded_password
---
apiVersion: v1
kind: Pod
metadata:
  name: redis
spec:
  containers:
    - name: redis
      image: redis:7
      envFrom:
        - secretRef:
            name: redis-secret
```

## Secret as a volume

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: go-program
spec:
  containers:
    - name: go-program
      image: go:1.20-bookworm
      ports:
        - containerPort: 80
      volumeMounts:
        - name: aws-secret-volume
          mountPath: /root/.aws
          readOnly: true
  volumes:
    - name: aws-secret-volume
      secret:
        secretName: aws-secret
```
