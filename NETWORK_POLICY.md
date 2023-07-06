# Network Policy

⚠️ The default rule is all `allow`

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
