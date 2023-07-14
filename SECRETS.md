# Secrets

## Read secret by key

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: crm
type: Opaque
data:
  redis-password: <base64-encoded-password>
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
              name: crm
              key: redis-password
```
