# Resource Quota

## Namespace quota

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: resource-quota-demo
  namespace: demo
spec:
  hard:
    requests.memory: 4Gi
    limits.memory: 4Gi
    requests.cpu: "4"
    limits.cpu: "4"
```
