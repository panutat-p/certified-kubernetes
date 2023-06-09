# Node Selection

## Node Selector

```shell
kubectl label nodes node-1 size=medium
kubectl label nodes node-2 size=large
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: nginx
  nodeSelector:
    size: medium
```

## Taint & Toleration

* `NoSchedule`
* `PreferNoSchedule`
* `NoExecute`

```shell
kubectl taint nodes node-1 type=master:NoSchedule
```

```shell
kubectl taint nodes node-1 type=master:NoSchedule-
```

```yaml
apiVersion: v1
kind: Node
metadata:
  name: node-1
spec:
  taints:
    - key: type
      value: master
      effect: NoSchedule
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:latest
  tolerations:
  - key: type
    operator: Equal
    value: slave
    effect: NoSchedule
```

## Node Affinity

* `requiredDuringSchedulingIgnoredDuringExecution`
* `preferredDuringSchedulingIgnoredDuringExecution`
* `requiredDuringSchedulingRequiredDuringExecution`
* `preferredDuringSchedulingRequiredDuringExecution`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:latest
  affinity:
    nodeAffinity:
      preferredDuringSchedulingIgnoredDuringExecution:
      - preference:
          matchExpressions:
          - key: size
            operator: NotIn
            values:
            - small
```
