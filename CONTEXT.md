# Context

https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters

Get cluster information
```shell
kubectl config get-contexts
```

```shell
kubectl config view | grep namespace
```

```shell
kubectl config set-context --current --namespace=default
```

## Resources

```shell
kubectl get all --namespace animal
```

```shell
kubectl get namespace
```

```shell
kubectl get service
```

```shell
kubectl get deployment
```
