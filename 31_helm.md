# Helm

https://helm.sh/docs/intro/using_helm

## Install NGINX chart via repository

https://charts.bitnami.com

```shell
helm search hub nginx
```

```shell
helm repo add bitnami https://charts.bitnami.com/bitnami
```

```shell
helm repo list
```

```shell
helm search repo nginx
```

```shell
helm install my-nginx bitnami/nginx
```

## Install NGINX chart via hub

https://artifacthub.io/packages/helm/bitnami/nginx

```shell
helm install my-nginx oci://registry-1.docker.io/bitnamicharts/nginx
```

```shell
helm delete my-nginx
```

## Create a new chart

```shell
helm create nginx-chart
```

```
nginx-chart/
  Chart.yaml          # A YAML file containing information about the chart
  LICENSE             # OPTIONAL: A plain text file containing the license for the chart
  README.md           # OPTIONAL: A human-readable README file
  values.yaml         # The default configuration values for this chart
  values.schema.json  # OPTIONAL: A JSON Schema for imposing a structure on the values.yaml file
  charts/             # A directory containing any charts upon which this chart depends.
  crds/               # Custom Resource Definitions
  templates/          # A directory of templates that, when combined with values, will generate valid Kubernetes manifest files.
  templates/NOTES.txt # OPTIONAL: A plain text file containing short usage notes
```
