# Multi-container Pods

## Example Elastic stack

https://bit.ly/2EXYdHf

https://github.com/kodekloudhub/event-simulator/tree/master

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: event-simulator
  namespace: demo
  labels:
    owner: admin
    app: event-simulator
spec:
  serviceAccountName: default
  containers:
    - name: app
      image: kodekloud/event-simulator
      volumeMounts:
        - mountPath: /log
          name: log-volume
    - name: sidecar
      image: kodekloud/filebeat-configured
      volumeMounts:
        - name: log-volume
          mountPath: /var/log/event-simulator
volumes:
  - name: log-volume
    hostPath:
      path: /var/log/webapp
      type: DirectoryOrCreate
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: elasticsearch
  namespace: demo
  labels:
    owner: admin
    app: elasticsearch
spec:
  serviceAccountName: default
  containers:
    - name: app
      image: elasticsearch:6.4.2
       ports:
         - containerPort: 9200
           protocol: TCP
         - containerPort: 9300
           protocol: TCP
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: kibana
  namespace: demo
  labels:
    owner: admin
    app: kibana
spec:
  serviceAccountName: default
  containers:
    - name: kibana
      image: kibana:6.4.2
      ports:
        - containerPort: 5601
          protocol: TCP
      env:
        - name: ELASTICSEARCH_URL
          value: http://elasticsearch:9200 
```
