# Basic Commands

one container per pod

## Context

```shell
kubectl config get-contexts
```

```shell
kubectl config view --minify | grep namespace:
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

## Utilities

```shell
kubectl logs -f my-pod
```

```shell
kubectl exec --stdin --tty my-pod -- /bin/sh 
```
