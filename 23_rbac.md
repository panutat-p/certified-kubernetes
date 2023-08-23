# RABC

## Role

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: developer
  namespace: demo
rules:
  - apiGroups:
      - ""
    resourceNames:
      - blue-pod
      - orange-pod
    resources:
      - pods
    verbs:
      - get
      - watch
      - create
      - delete
  - apiGroups:
      - apps
    resources:
      - deployments
    verbs:
      - create
      - list
```

```yaml

```
