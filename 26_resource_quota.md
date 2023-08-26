# Resource Quota

https://kubernetes.io/docs/concepts/policy/resource-quotas

* `ResourceQuota` provides constraints that limit aggregate resource consumption per namespace
* The administrator creates one `ResourceQuota` for each namespace
* Available quotas
  * cpu
  * memory
  * number of pods
  * number of deployemnts
  * number of services
  * number of `ConfigMap`
  * number of `Secret`
  * number of `PersistentVolumeClaim`

## Limiting `cpu` & `memory`

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
