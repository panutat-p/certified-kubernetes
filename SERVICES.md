# Services

## ClusterIP

* The ClusterIP is an internal virtual IP address assigned to the service
* Allows other services within the cluster to communicate with it
* Provides a stable endpoint for internal communication

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
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-server
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web-server
  template:
    metadata:
      labels:
        app: web-server
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
  name: web-server-service
spec:
  type: LoadBalancer
  selector:
    app: web-server
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```
