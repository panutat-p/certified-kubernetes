# RBAC Authorization

https://kubernetes.io/docs/reference/access-authn-authz/rbac

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

## Role Binding

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: developer-binding
  namespace: demo
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: developer
subjects:
  - apiGroup: rbac.authorization.k8s.io
    kind: User
    name: dev-user
```
