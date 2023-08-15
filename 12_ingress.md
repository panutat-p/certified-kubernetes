# Ingress Networking

## Ingress Resources

*  Acts as a layer of abstraction between the external traffic and the services running in the cluster
*  Define rules to route traffic to specific services or pods based on criteria
*  Create an ingress alone will not work because Kubernetes does not have `Ingress Controller` out of the box

## Example

Why we need `rewrite-target`?\
https://www.udemy.com/course/certified-kubernetes-application-developer/learn/lecture/16716434#questions

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: sample-ingress
  namespace: demo
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
spec:
  rules:
    - http:
        paths:
          - path: /wear
            pathType: Prefix
            backend:
              service:
                name: wear-service
                port:
                  number: 8080
          - path: /watch
            pathType: Prefix
            backend:
              service:
                name: video-service
                port:
                  number: 8080
```
