# Secrets

Guideline\
https://opensource.com/article/19/6/introduction-kubernetes-secrets-and-configmaps

## Read secret by key

`env.name` will be a environment variable name\
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
