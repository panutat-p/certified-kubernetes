# Basic Commands

https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands

## Cluster

```shell
kubectl config get-contexts
```

```shell
kubectl api-resources
```

## Help

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

## Get

```shell
kubectl get all -A
```

```shell
kubectl get all -l app=nginx
```

```shell
kubectl get ns
```

```shell
kubectl get sa
```

```shell
kubectl get ing
```

```shell
kubectl get crd
```

```shell
kubectl get netpol
```

```shell
kubectl get role
```

```shell
kubectl get rolebinding
```

## Create

```shell
kubectl create deploy web --image nginx:1.25 --port 80 --replicas 3 -o yaml
```

```shell
kubectl create svc clusterip nginx-service --tcp 80:80 nginx:1.25 --replicas 3 -o yaml
```

```shell
kubectl run nginx-pod --image nginx:1.25 --port 80 --labels app=nginx -o yaml
```

```shell
kubectl run ubuntu --image ubuntu:23.10 --labels app=ubuntu --command -- sleep infinity
```

```shell
kubectl expose pod nginx-pod --name nginx-service --type ClusterIP --port 80 --target-port 80
```

```shell
kubectl create ing item --class default --rule example.com/items*=item-service:80
kubectl create ing payment --class default --rule /pay=pay-service:8282
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

## Manifest file

```shell
kubectl apply -f file.yaml
```

```shell
kubectl create -f file.yaml
```

```shell
kubectl delete -f file.yaml
```
