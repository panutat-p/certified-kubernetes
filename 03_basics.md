# Basics

## Documentations

```shell
kubectl api-resources
```

```shell
kubectl explain pod.spec --recursive | less
kubectl explain pod.spec.containers --recursive | less
kubectl explain pod.spec.volumes --recursive | less
```

## Resources

```shell
kubectl get all -n reddit
```

```shell
kubectl describe service/name -n reddit
kubectl describe deploy/name -n reddit
kubectl describe pod/name -n reddit
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
kubectl scale deploy/name--replicas=3 -n dev
```

```shell
kubectl get deploy/name -n dev
```

Listen on port 5000 on the local machine and forward to port 6000 on my-pod
```shell
kubectl port-forward my-pod 5000:6000
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

## Manifest files

```shell
kubectl create deployment d-sample --image=nginx --dry-run=client -o yaml > d-sample.yaml
```

```shell
kubectl create pod p-sample --image=nginx --port=80 --dry-run=client -o yaml > p-sample.yaml
```

## Edit running resources

When try to edit a image of a pod
```shell
kubectl edit pod/web-server-1
```

> A copy of your changes has been stored to "/tmp/kubectl-edit-<random>.yaml"

```shell
cd /tmp
kubectl replace --force -f ./kubectl-edit-<random>.yaml
```

Export pod manifest file
```shell
kubectl get pod/web-server-1 -o yaml > web-server-1.yaml
nano web-server-1.yaml
```

```shell
kubectl delete pod/web-server-1.yaml
kubectl create -f web-server-1.yaml
```
