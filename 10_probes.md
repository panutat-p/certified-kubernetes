# Probes

https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes

## HTTP probes

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
      image: nginx
      ports:
        - containerPort: 80
      livenessProbe:
        httpGet:
          path: /
          port: 80
        initialDelaySeconds: 15
        periodSeconds: 10
      readinessProbe:
        httpGet:
          path: /
          port: 80
        initialDelaySeconds: 5
        periodSeconds: 5
```

## Local file probes

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: busybox
  namespace: demo
  labels:
    owner: admin
    app: busybox
spec:
  serviceAccountName: default
  containers:
    - name: busybox
      image: busybox:1.36
      command: ['sh']
      args: ['-c', 'sleep 5; touch /tmp/healthy']
      livenessProbe:
        exec:
          command: [ 'cat', '/tmp/healthy']
        initialDelaySeconds: 10
        periodSeconds: 10
      readinessProbe:
        exec:
          command: [ 'cat', '/tmp/healthy']
        initialDelaySeconds: 5
        periodSeconds: 5
```

## TCP probes

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: redis
  namespace: demo
  labels:
    owner: admin
    app: redis
spec:
  serviceAccountName: default
  containers:
    - name: redis
      image: redis:7
      livenessProbe:
        tcpSocket:
          port: 6379
        initialDelaySeconds: 15
        periodSeconds: 10
      readinessProbe:
        tcpSocket:
          port: 6379
        initialDelaySeconds: 10
        periodSeconds: 5
```
