# Multi-container Pods

## Sidecar example

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-with-logger
spec:
  containers:
    - name: nginx
      image: nginx
      ports:
        - containerPort: 80
    - name: log-agent
      image: log-agent-image
      ports:
        - containerPort: 8080
```

## Init Container

```yaml

```
