# Edit running resources

## Update image

```shell
kubectl edit pod/web-server-1
```

## Update other fields

```shell
kubectl edit pod/web-server-1
```

`A copy of your changes has been stored to "/tmp/kubectl-edit-<random>.yaml"`

```shell
cd /tmp
kubectl replace --force -f ./kubectl-edit-<random>.yaml
```
