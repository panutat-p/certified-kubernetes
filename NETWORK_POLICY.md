# Network Policy

https://kubernetes.io/docs/concepts/services-networking/network-policies

* ⚠️ By default, a pod is non-isolated for ingress and egress
* NetworkPolicies control traffic flow at the IP address or port level (OSI layer 3 or 4)
* NetworkPolicies apply to a connection with a pod on one or both ends

## Example 1: Allow Nginx traffic to the pod

Use a selector to specify what traffic is allowed to and from the pod

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
