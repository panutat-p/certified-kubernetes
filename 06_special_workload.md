# Special Workload Resources

## StatefulSets

* `volumeClaimTemplates` in a StatefulSet allows for the dynamic creation of PVCs for each replica (Pod) based on the specified template
* The association between the PVC and the PV is maintained even if the Pod is deleted or rescheduled
* These PVCs are bound to underlying PVs, and the PVCs are retained and reused across Pod rescheduling and updates, ensuring data persistence for stateful applications
* If the PV is dynamically provisioned, it might be associated with different underlying storage across Pod rescheduling, but the PVC and Pod will still maintain the same persistent identity

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
