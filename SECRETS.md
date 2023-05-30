# Secrets

`secret.yaml`

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: crm
type: Opaque
data:
  redis-password: <base64-encoded-password>
```

`redis.yaml`

```yaml
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
              name: crm
              key: redis-password
```

```shell
kubectl apply -f secret.yaml -f redis.yaml
```
