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

## Ingress Resources

*  Acts as a layer of abstraction between the external traffic and the services running in the cluster
*  Define rules to route traffic to specific services or pods based on criteria
*  Create an ingress alone will not work because Kubernetes does not have `Ingress Controller` out of the box

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
spec:
  rules:
    - host: example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: example-service
                port:
                  number: 80
```

## Ingress Controller

* Kubernetes does not come with Ingress Controller by default
* Example: `GKE Ingress`, `Nginx`, `Contour`, `HAProxy`, `Traefik`, `Istio`
