# Volumes

* Any data written to this directory within the container will be stored in the `emptyDir` volume associated with the pod
* This `emptyDir` volume is non-persistent and will lose its data if the pod is terminated or rescheduled to a different node.

`redis.yaml`

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

```shell
kubectl create -f redis.yaml
```
