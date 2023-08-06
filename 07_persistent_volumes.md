# Persistent Volumes

## Static PV

An administrator creates a number of PVs that carry the details of the real storage, which is available for use by cluster users.

## Dynamic PV

When none of the static PVs match a PVC, the cluster may try to dynamically provision a PV specially for the PVC.

https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage
