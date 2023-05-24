# SET UP

```shell
kubectl version --output=yaml
kubectl config view
kubectl config get-contexts
```

```shell
kubectl config view --minify | grep namespace:
kubectl config set-context --current --namespace=default
```
