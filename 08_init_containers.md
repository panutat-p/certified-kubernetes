# Init Containers

https://kubernetes.io/docs/concepts/workloads/pods/init-containers

* Each init container must exit successfully before the next container starts
* If a container fails to start due to the runtime or exits with failure, it is retried according to the Pod `restartPolicy`
* A pod cannot be Ready until all init containers have succeeded

## Example Nginx

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  namespace: demo
  labels:
    owner: admin
    app: nginx
spec:
  serviceAccountName: default
  containers:
    - name: nginx
      image: nginx:1.25
      ports:
        - containerPort: 80
      volumeMounts:
        - name: conf-volume
          mountPath: /etc/nginx
  initContainers:
    - name: download-nginx-conf
      image: busybox:1.36
      command: ["wget", "https://github.com/nginx/nginx/blob/master/conf/nginx.conf", "-O", "/tmp/nginx.conf"]
      volumeMounts:
        - name: conf-volume
          mountPath: /etc/nginx
  volumes:
    - name: conf-volume
      emptyDir: {}
```
