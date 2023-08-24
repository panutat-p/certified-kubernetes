# Limit Range

https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/memory-default-namespace

* Container `resources.limit` will override both limit values of `LimitRange`
* Container `resources.requests` will override request limit value of `LimitRange`

## Simple

```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: demo-limit-range
  namespace: demo
spec:
  limits:
    - type: Container
      defaultRequest:
        cpu: 100m
        memory: 100Mi
      defaultLimit:
        cpu: 1
        memory: 1Gi
      maxLimitRequestRatio:
        cpu: 4
        memory: 2
      min:
        cpu: 100m
        memory: 50Mi
      max:
        cpu: 2
        memory: 2Gi
```
