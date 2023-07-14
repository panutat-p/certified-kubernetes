# Secrets

## Read secret by key

In Kubernetes, environment variables are case-sensitive.\
Therefore, the environment variable name defined in the env section must match the key in the secret exactly for the value to be fetched correctly.

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: redis-secret
type: Opaque
data:
  REDIS_HOST: MTAuMC4xLjE=
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
        - name: REDIS_HOST
          valueFrom:
            secretKeyRef:
              name: redis-secret
              key: REDIS_HOST
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
  REDIS_HOST: MTAuMC4xLjE=
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
