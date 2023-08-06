# Storages

https://kubernetes.io/docs/concepts/storage

## Empty directory

* Any data written to this directory within the container will be stored in the `emptyDir` volume associated with the pod
* This `emptyDir` volume is non-persistent and will lose its data if the pod is terminated or rescheduled to a different node.

## Nginx

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
      emptyDir: {}
```

```shell
kubectl cp ~/index.html nginx-pod:/usr/share/nginx/html/index.html
```

## ConfigMap as ENV

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

# Secrets

Guideline\
https://opensource.com/article/19/6/introduction-kubernetes-secrets-and-configmaps

## Read secret by key

`env.name` will be the environment variable name\
`secretKeyRef.key` need to match the Secret data

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
  name: redis-pod
spec:
  containers:
    - name: redis
      image: redis
      env:
        - name: REDIS_PASSWORD
          valueFrom:
            secretKeyRef:
              name: redis-secret
              key: redis-password
```

## Read whole secret object

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
  name: redis-pod
spec:
  containers:
    - name: redis
      image: redis
      envFrom:
        - secretRef:
            name: redis-secret
```

## Mount secret object

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
