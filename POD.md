# Kubernetes Definition Files

## Basic

```shell
kubectl apply -f file.yaml
```

```shell
kubectl create -f file.yaml
```

```shell
kubectl delete -f file.yaml
```

```shell
kubectl describe service/name -n dev
kubectl describe deployment/name -n dev
```

```shell
kubectl create deployment d-sample --image=nginx --dry-run=client -o yaml > d-sample.yaml
```

```shell
kubectl create pod p-sample --image=nginx --port=80 --dry-run=client -o yaml > p-sample.yaml
```

```shell
kubectl scale deploy/name--replicas=3 -n dev
```

```shell
kubectl get deploy/name -n dev
```

## Utilities

```shell
kubectl logs -f my-pod
```

```shell
kubectl exec my-pod -- ls
```

```shell
kubectl exec --stdin --tty my-pod -- /bin/sh 
```

Listen on port 5000 on the local machine and forward to port 6000 on my-pod
```shell
kubectl port-forward my-pod 5000:6000
```


## Edit running resources

When try to edit a image of a pod
```shell
kubectl edit pod/web-server-1
```

`A copy of your changes has been stored to "/tmp/kubectl-edit-<random>.yaml"`

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

## Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  namespace: dev
  labels:
    owner: panda
    app: nginx
spec:
  containers:
    - name: nginx
      image: nginx:latest
      ports:
        - containerPort: 80
```

## Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: dev
spec:
  replicas: 3
  selector:
    matchLabels:
      owner: panda
      app: nginx
  template:
    metadata:
      labels:
        owner: panda
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          ports:
            - containerPort: 80
```
