# Basic Commands

https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands

## Help

```shell
kubectl api-resources
```

```shell
kubectl create -h
kubectl run -h
kubectl expose -h
kubectl scale -h
kubectl exec -h
```

```shell
kubectl explain pod.spec | less
kubectl explain pod.spec.containers --recursive | less
```

```shell
kubectl explain deploy.spec.template | less
kubectl explain deploy.spec.template.spec --recursive | less
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
kubectl run ubuntu --image ubuntu:23.10 --labels app=ubuntu --command -- sleep infinity
```

```shell
kubectl expose pod nginx-pod --type=ClusterIP --port=80 --target-port=80 --name=nginx-service
```

## Dry run

```shell
kubectl create deploy httpd --image httpd:2.4 --replicas 3 -o yaml --dry-run=client
```

## Resources

```shell
kubectl describe svc/name
kubectl describe deploy/name
kubectl describe pod/name
```

```shell
kubectl get pod/name -o yaml
```

```shell
kubectl get pod/name -o yaml > pod.yaml
```

## Execute commands

```shell
kubectl exec my-pod -- ls
```

```shell
kubectl exec --stdin --tty my-pod -- /bin/bash
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
