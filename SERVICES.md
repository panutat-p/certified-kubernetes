# Services

https://kubernetes.io/docs/concepts/services-networking/service

* A Service acts as a stable endpoint for accessing your application and provides a way to load balance traffic among the pods
* If you use a Deployment to run your app, that Deployment can create and destroy Pods dynamically
* From one moment to the next, you don't know how many of those Pods are working and healthy; you might not even know what those healthy Pods are named.

## ClusterIP

* An IP address in the cluster
* Allows other services within the cluster to communicate with it
* Internal communication

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web-app
  template:
    metadata:
      labels:
        app: web-app
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: webapp-service
spec:
  type: ClusterIP
  selector:
    app: web-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```

## NodePort

* `nodePort` is in range 30000 - 32767 (implicit)
* Forward port of the node to the port of the pod
* To route traffic to the pods created by the Deployment,\
the selector field in the Service resource must match the labels specified in the Deployment's pod template

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: NodePort

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          ports:
            - containerPort: 80
```

## LoadBalancer

* The LoadBalancer service type is typically used in cloud environments
* The external load balancer or the cloud load balancer automatically assigns an external IP address (or hostname) that clients can use to access the service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: LoadBalancer

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          ports:
            - containerPort: 80
```
