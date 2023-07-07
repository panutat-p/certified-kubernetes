# Network Policy

https://kubernetes.io/docs/concepts/services-networking/network-policies

⚠️ By default, a pod is non-isolated for ingress and egress

## Example 1: Allow Nginx traffic to the pod

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-nginx-to-httpd
spec:
  podSelector:
    matchLabels:
      app: httpd
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: nginx
    ports:
    - protocol: TCP
      port: 80
```
