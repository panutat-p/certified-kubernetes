# Ingress Networking

## Ingress Resources

*  Acts as a layer of abstraction between the external traffic and the services running in the cluster
*  Define rules to route traffic to specific services or pods based on criteria
*  Create an ingress alone will not work because Kubernetes does not have `Ingress Controller` out of the box
*  Now, in k8s version 1.20+ we can create an Ingress resource in the imperative way like this\
`kubectl create ingress example-ingress --rule="example.com/items*=example-service:80"`

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
          - path: /items
            pathType: Prefix
            backend:
              service:
                name: example-service
                port:
                  number: 80
```

## Ingress Controller

* Kubernetes does not come with Ingress Controller by default
* Popular choices: `GKE Ingress`, `Nginx`, `Contour`, `HAProxy`, `Traefik`, `Istio`
* Example nginx ingress controller
  * https://docs.nginx.com/nginx-ingress-controller/installation/installation-with-manifests
  * https://github.com/nginxinc/kubernetes-ingress/blob/main/deployments/deployment/nginx-ingress.yaml

## Full Example

`$(POD_NAMESPACE)` is an environment variable that represents the namespace in which the NGINX Ingress Controller pod is running.\
This allows the Ingress Controller to automatically retrieve the NGINX configuration ConfigMap from the same namespace without hard-coding the namespace value.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-ingress-service
  namespace: ingress-nginx
spec:
  type: LoadBalancer
  selector:
    app: nginx-ingress
  ports:
    - name: http
      port: 80
      targetPort: http
    - name: https
      port: 443
      targetPort: https
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-ingress-controller
  namespace: ingress-nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx-ingress
  template:
    metadata:
      labels:
        app: nginx-ingress
    spec:
      containers:
        - name: nginx-ingress-controller
          image: quay.io/kubernetes-ingress-controller/nginx-ingress-controller:0.27.1
          args:
            - /nginx-ingress-controller
            - --configmap=$(POD_NAMESPACE)/nginx-configuration
          env:
            - name: POD_NAME
              valueFrom:
                fieldRef:
                  fieldPath: metadata.name
            - name: POD_NAMESPACE
              valueFrom:
                fieldRef:
                  fieldPath: metadata.namespace
          ports:
            - name: http
              containerPort: 80
            - name: https
              containerPort: 443
          volumeMounts:
            - name: nginx-configuration
              mountPath: /etc/nginx/nginx.conf
              subPath: nginx.conf
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: nginx-configuration
  namespace: ingress-nginx
data:
  nginx.conf: |
    events {
      worker_connections 1024;
    }
  
    http {
      server {
        listen 80;
        server_name example.com;
  
        location / {
          proxy_pass http://backend-service;
        }
      }
    }
```

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
```

```yaml
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
