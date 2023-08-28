# ENV

https://kubernetes.io/docs/tasks/inject-data-application/define-interdependent-environment-variables

https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container

## Static ENV

```
apiVersion: v1
kind: Pod
metadata:
  name: envar-demo
  namespace: demo
  labels:
    app: busybox
spec:
  containers:
    - name: envar-demo-container
      image: busybox:1.36
      env:
        - name: DEMO_GREETING
          value: "Hello from the environment"
        - name: DEMO_FAREWELL
          value: "Such a sweet sorrow"
```
