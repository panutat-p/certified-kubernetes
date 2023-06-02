# Volumes

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

## Redis

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: redis-pod
spec:
  containers:
    - name: redis
      image: redis
      ports:
        - containerPort: 6379
      volumeMounts:
        - name: redis-data
          mountPath: /data
  volumes:
    - name: redis-data
      emptyDir: {}
```
