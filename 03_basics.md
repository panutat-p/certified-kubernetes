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

https://kubernetes.io/docs/reference/kubectl/cheatsheet/#kubectl-context-and-configuration

```bash
#!/bin/bash
kn() {
  [ "$1" ] && kubectl config set-context --current --namespace $1 || kubectl config view --minify | grep namespace
}
```

## Quick run

```shell
kubectl create deploy web --image nginx:1.25 --port 80 --replicas 3 -o yaml
```

```shell
kubectl create svc clusterip nginx-service --tcp 80:80 nginx:1.25 --replicas 3 -o yaml
```

```shell
kubectl run nginx-pod --image=nginx:1.25 --port 80 --labels app=nginx -o yaml
```

```shell
kubectl expose pod nginx-pod --type=ClusterIP --port=80 --target-port=80 --name=nginx-service
```

## Dry run

```shell
kubectl create deploy httpd --image httpd --replicas 3 -o yaml --dry-run=client
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
