# Ingress Networking

https://kubernetes.io/docs/concepts/services-networking/ingress

* Acts as a layer of abstraction between the external traffic and the services running in the cluster
* Define rules to route traffic to specific services or pods based on criteria
* Creating an ingress alone will not work because Kubernetes does not have `Ingress Controller` out of the box

## Default back-end

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: all-ingress
  namespace: demo
spec:
  defaultBackend:
    service:
      name: test
      port:
        number: 80
```

## Nginx rewrite target

* Documentation for the Ingress NGINX Controller
  * https://kubernetes.github.io/ingress-nginx/examples
* `Rewrite` behavior of `Nginx Ingress` cannot be replicated by `GCE Ingress`
  * https://stackoverflow.com/a/71033160
* Example of path-based routing with `rewrite-target`
  * https://www.udemy.com/course/certified-kubernetes-application-developer/learn/lecture/16716434#questions

```yaml
annotations:
  nginx.ingress.kubernetes.io/rewrite-target: /
```

## Any host

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: sample-ingress
  namespace: demo
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  defaultBackend: {}
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

## Host and path

```yaml
kind: Ingress
apiVersion: networking.k8s.io/v1
metadata:
  name: ingress-vh-routing
  namespace: demo
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  defaultBackend: {}
  rules:
    - host: watch.ecom-store.com
      http:
        paths:
          - pathType: Exact
            path: /video
            backend:
              service:
                name: video-service
                port:
                  number: 8080
    - host: apparels.ecom-store.com
      http:
        paths:
          - pathType: Exact
            path: /wear
            backend:
              service:
                name: apparels-service
                port:
                  number: 8080
```
