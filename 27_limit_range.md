# Limit Range

https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/memory-default-namespace

* Container `resources` may override `LimitRange` values
* Creating a pod that does not meet the `min` or `max` is not allowed

## Simple

```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: resource-limit-range
  namespace: demo
spec:
  limits:
    - type: Container
      defaultRequest:
        cpu: 100m
        memory: 100Mi
      default:
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
