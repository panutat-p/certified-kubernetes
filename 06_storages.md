# Storages

https://kubernetes.io/docs/concepts/storage

## Volume

* Any data written to this directory within the container will be stored in the `emptyDir` volume associated with the pod
* This `emptyDir` volume is non-persistent and will lose its data if the pod is terminated or rescheduled to a different node.

## Nginx

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
    - name: nginx
      image: nginx
      ports:
        - containerPort: 80
      volumeMounts:
        - name: html-volume
          mountPath: /usr/share/nginx/html
  volumes:
    - name: html-volume
      emptyDir: {}
```

```shell
kubectl cp ~/index.html nginx-pod:/usr/share/nginx/html/index.html
```

## Persistent Volumes

### Static PV

An administrator creates a number of PVs that carry the details of the real storage, which is available for use by cluster users.

### Dynamic PV

When none of the static PVs match a PVC, the cluster may try to dynamically provision a PV specially for the PVC.

https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage
