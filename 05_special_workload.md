# Special Workload Resources

## StatefulSets

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: website
  namespace: reddit
spec:
  selector:
    matchLabels:
      owner: panda
      app: website
  replicas: 3
  template:
    metadata:
      labels:
        owner: panda
        app: website
    spec:
      containers:
      - name: nginx
        image: "nginx:1.25-bookworm"
        ports:
        - containerPort: 80
          name: web
        volumeMounts:
        - name: www
          mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
  - metadata:
      name: www
    spec:
      accessModes: [ "ReadWriteOnce" ]
      storageClassName: ""
      resources:
        requests:
          storage: 1Gi
```

## DaemonSet

```yaml

```
