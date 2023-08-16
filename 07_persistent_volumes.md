# Persistent Volumes

https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage

* Static PV
  * An administrator creates a number of PVs that carry the details of the real storage, which is available for use by cluster users.
* Dynamic PV
  * When none of the static PVs match a PVC, the cluster may try to dynamically provision a PV specially for the PVC.
* A PVC with its `storageClassName` set equal to `""` is always interpreted to be requesting a PV with no class

## PV host path, PVC, pod

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-log
  namespace: demo
  labels:
    app: kodekloud
spec:
  storageClassName: ""
  persistentVolumeReclaimPolicy: Retain
  capacity:
    storage: 100Mi
  accessModes:
    - ReadWriteMany
  hostPath:
    path: /pv/log
```

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: claim-log-1
spec:
  volumeMode: Filesystem
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 50Mi
```

```yaml

```
