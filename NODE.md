## Node Selector

```shell
kubectl label nodes node-1 app=auth
kubectl label nodes node-2 app=product
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
    app: product
```

## Taint

```shell

```

## Toleration

```shell

```

## Node Affinity

```shell

```
