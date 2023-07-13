# Kustomize

https://kubectl.docs.kubernetes.io/references/kustomize/kustomization

```shell
kustomize edit fix
```

```shell
kustomize create --namespace=reddit --resources=deployment.yaml,service.yaml --labels=app:gateway
```

## Example: Nginx

`base/kustomization.yaml`
```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
  - deployment.yaml
```

`base/deployment.yaml`
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
```

`dev/kustomization.yaml`
```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
bases:
  - ../base
patches:
  - target:
      kind: Deployment
      name: nginx-deployment
    path: deployment.yaml
```

`dev/deployment.yaml`
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  template:
    spec:
      containers:
        - name: nginx
          image: nginx:1.17.10
          ports:
            - containerPort: 80
```

```shell
kubectl kustomize ./dev
```

```shell
kubectl apply -k ./dev
```

```shell
kubectl delete -k ./dev
```

## Example 2: ConfigMap generator

https://kubectl.docs.kubernetes.io/references/kustomize/kustomization/configmapgenerator

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

# These labels are added to all configmaps
generatorOptions:
  disableNameSuffixHash: true
  labels:
    type: generated
  annotations:
    note: generated

configMapGenerator:
- name: my-config
  envs:
  - config.env
```

## Example 3: Secret Generator

https://kubectl.docs.kubernetes.io/references/kustomize/kustomization/secretgenerator

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

# These labels are added to all configmaps
generatorOptions:
  disableNameSuffixHash: true
  labels:
    type: generated
  annotations:
    note: generated

secretGenerator:
- name: app-tls
  files:
  - tls.crt
  - tls.key
  type: "kubernetes.io/tls"
- name: my-secret
  type: Opaque
  namespace: apps
  envs:
  - secret.env
```
