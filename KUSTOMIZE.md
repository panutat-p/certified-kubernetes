# Kustomize

https://kubectl.docs.kubernetes.io/references

```shell
kustomize create --namespace=reddit --resources=deployment.yaml,service.yaml --labels=app:gateway
```

## Template

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

resources:
- {pathOrUrl}
- ...

generators:
- {pathOrUrl}
- ...

transformers:
- {pathOrUrl}
- ...

validators:
- {pathOrUrl}
- ...
```
