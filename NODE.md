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

## Taint

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

## Toleration

```shell

```

## Node Affinity

```shell

```
