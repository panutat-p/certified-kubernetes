# Edit running resources

## Update pod image

```shell
kubectl edit pod/web-server-1
```

## Replace pod

```shell
kubectl edit pod/web-server-1
```

`A copy of your changes has been stored to "/tmp/kubectl-edit-<random>.yaml"`

```shell
cd /tmp
kubectl replace --force -f ./kubectl-edit-<random>.yaml
```

## Recreate pod

```shell
kubectl get pod/web-server-1 -o yaml > web-server-1.yaml
nano web-server-1.yaml
```

```shell
kubectl delete pod/web-server-1.yaml
kubectl create -f web-server-1.yaml
```
