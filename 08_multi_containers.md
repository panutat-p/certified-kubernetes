# Multi-container Pods

## Sidecar example

a web application and logging agent, both containers are expected to stay alive at all times.
The process running in the log agent container is expected to stay alive as long as the web application is running.
If any of them fails, the POD restarts.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-with-logger
spec:
  containers:
    - name: nginx
      image: nginx
      ports:
        - containerPort: 80
    - name: log-agent
      image: log-agent-image
      ports:
        - containerPort: 8080
```

## Init Container

Each init container is run one at a time in sequential order.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: config-pod
spec:
  containers:
  - name: main-container
    image: main-container-image:latest
    # Main container specification
    volumeMounts:
    - name: config-volume
      mountPath: /config

  initContainers:
  - name: fetch-config
    image: busybox:latest
    command: ["sh", "-c", "wget -O /config/config-file.txt https://example.com/config/config-file.txt"]
    volumeMounts:
    - name: config-volume
      mountPath: /config

  volumes:
  - name: config-volume
    emptyDir: {}
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
spec:
  containers:
  - name: myapp-container
    image: busybox:1.28
    command: ['sh', '-c', 'echo The app is running! && sleep 3600']
  initContainers:
  - name: init-myservice
    image: busybox:1.28
    command: ['sh', '-c', 'until nslookup myservice; do echo waiting for myservice; sleep 2; done;']
  - name: init-mydb
    image: busybox:1.28
    command: ['sh', '-c', 'until nslookup mydb; do echo waiting for mydb; sleep 2; done;']
```
