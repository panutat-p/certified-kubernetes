# Basics

https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands

## Help

```shell
kubectl api-resources
```

```shell
kubectl explain pod.spec --recursive | less
kubectl explain pod.spec.containers --recursive | less
kubectl explain pod.spec.volumes --recursive | less
```

## Context

```shell
kubectl config view | grep namespace
```

```shell
kubectl config set-context --current --namespace=reddit
```

## Quick run

```shell
kubectl run nginx --image=nginx -n reddit
```

Listen on port 4000 on the local machine
```shell
kubectl port-forward nginx 4000:80
```

## Resources

```shell
kubectl get all -n reddit
```

```shell
kubectl logs -f my-pod -n reddit
```

```shell
kubectl exec my-pod -n reddit -- ls
```

```shell
kubectl exec --stdin --tty my-pod -n reddit -- /bin/bash
```

```shell
kubectl get deploy/name -n dev
```

## Create / Update / Delete

```shell
kubectl apply -f file.yaml
```

```shell
kubectl create -f file.yaml
```

```shell
kubectl delete -f file.yaml
```

## Inspect

```shell
kubectl describe svc/name -n reddit
kubectl describe deploy/name -n reddit
kubectl describe pod/name -n reddit
```

```shell
kubectl get pod/nginx -o yaml -n reddit > nginx.yaml
```
