# Network Policy

https://kubernetes.io/docs/concepts/services-networking/network-policies

* NetworkPolicy requires a plugin, creating a NetworkPolicy resource without a controller that implements it will have no effect
* ⚠️ By default, all pods are non-isolated for ingress and egress
* NetworkPolicy control traffic flow at the IP address or port level (OSI layer 3 or 4)
* NetworkPolicy apply to a connection with a pod on one or both ends
* 🦊 NetworkPolicy is applied at the namespace level without requiring any changes to the pods themselves

## Simple network policy between httpd pod and nginx pod

```yaml
kubectl run nginx-01 --image nginx:1.25 -l app=nginx -o yaml
kubectl run nginx-02 --image nginx:1.25 -l app=nginx -o yaml
kubectl run httpd-01 --image httpd:2 -l app=httpd -o yaml
kubectl run httpd-02 --image httpd:2 -l app=httpd -o yaml
```

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: nginx-policy
  namespace: demo
spec:
  podSelector:
    matchLabels:
      app: nginx
  policyTypes:
    - Ingress
    - Egress
  ingress:
    - from:
        - podSelector:
            matchLabels:
              app: httpd
  egress:
    - to:
        - podSelector:
            matchLabels:
              app: httpd
```

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: httpd-policy
  namespace: demo
spec:
  podSelector:
    matchLabels:
      app: httpd
  policyTypes:
    - Ingress
    - Egress
  ingress:
    - from:
        - podSelector:
            matchLabels:
              app: nginx
      ports: {}
  egress:
    - to:
        - podSelector:
            matchLabels:
              app: nginx
      ports: {}
```

## Allow internal pod to access payroll pod and MySQL pod

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: internal-policy
  namespace: demo
spec:
  podSelector:
    matchLabels:
      name: internal
  policyTypes:
    - Egress
  egress:
    - to:
      - podSelector:
          matchLabels:
            name: payroll
      ports:
        - protocol: TCP
          port: 8080
    - to:
      - podSelector:
          matchLabels:
            name: mysql
      ports:
        - protocol: TCP
          port: 3306
```

## Block IP address range

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: test-network-policy
  namespace: demo
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
