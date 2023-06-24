# Ingress Networking

## Load Balancer Service

* `LoadBalancer` is typically used in production environments
* `LoadBalancer` provides an external IP address (typically from a cloud provider's load balancer) to access the service
* The cloud provider sets up a load balancer that forwards external traffic to the service
* `nodePort` is in range 30000 - 32767 (implicit)

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
