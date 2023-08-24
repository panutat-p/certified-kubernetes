# Persistent Volumes

https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage

* Static PV\
An administrator creates a number of PVs that carry the details of the real storage, which is available for use by cluster users.
* Dynamic PV\
When none of the static PVs match a PVC, the cluster may try to dynamically provision a PV specially for the PVC.
* A PVC with its `storageClassName` set equal to `""` is always interpreted to be requesting a PV with no class
* The `Retain` reclaim policy allows for manual reclamation of the resource.\
When the PVC is deleted, the PV still exists and the volume is considered `released`\
But it is not yet available for another claim because the previous claimant's data remains on the volume

## PV host path -> PVC -> pod

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: log-storage
  namespace: demo
spec:
  storageClassName: manual
  persistentVolumeReclaimPolicy: Retain
  accessModes:
    - ReadWriteMany
  hostPath:
    path: /pv/log
  capacity:
    storage: 100Mi
```

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: log-claim
  namespace: demo
spec:
  storageClassName: manual
  volumeMode: Filesystem
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 50Mi
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: event-simulator
  namespace: demo
  labels:
    app: event-simulator
spec:
  containers:
    - name: webapp
      image: kodekloud/event-simulator
      volumeMounts:
        - name: log-volume
          mountPath: /log
  volumes:
    - name: log-volume
      persistentVolumeClaim:
        claimName: log-claim
```
