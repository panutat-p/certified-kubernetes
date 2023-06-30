# Ingress Networking

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

## Full Example

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
spec:
  rules:
    - host: example.com
      http:
        paths:
          - path: /v1
            pathType: Prefix
            backend:
              service:
                name: nginx-service
                port:
                  number: 80
          - path: /v2
            pathType: Prefix
            backend:
              service:
                name: nginx-service
                port:
                  number: 80
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
spec:
  replicas: 1
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
