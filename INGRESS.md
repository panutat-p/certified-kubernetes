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
