# Multi-container Pods

## Ambassador

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
spec:
  containers:
    - name: webapp
      image: myapp-webapp-image
      ports:
        - containerPort: 80
    - name: log-agent
      image: myapp-log-agent-image
      ports:
        - containerPort: 8080
```

## Adapter

```yaml

```

## Sidecar

```yaml

```
