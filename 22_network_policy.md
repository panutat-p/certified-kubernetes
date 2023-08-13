# Network Policy

https://kubernetes.io/docs/concepts/services-networking/network-policies

* ⚠️ By default, a pod is non-isolated for ingress and egress
* NetworkPolicies control traffic flow at the IP address or port level (OSI layer 3 or 4)
* NetworkPolicies apply to a connection with a pod on one or both ends

## Example 1: Block all traffic

```yaml
---
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-ingress
spec:
  podSelector: {}
  policyTypes:
  - Ingress
```

## Example 2: Allow Nginx traffic to the pod

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

## Example 3: Block IP Addresses

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: test-network-policy
  namespace: default
spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes:
    - Ingress
    - Egress
  ingress:
    - from:
        - ipBlock:
            cidr: 172.17.0.0/16
            except:
              - 172.17.1.0/24
        - namespaceSelector:
            matchLabels:
              project: myproject
        - podSelector:
            matchLabels:
              role: frontend
      ports:
        - protocol: TCP
          port: 6379
  egress:
    - to:
        - ipBlock:
            cidr: 10.0.0.0/24
      ports:
        - protocol: TCP
          port: 5978
```
