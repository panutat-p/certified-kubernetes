# Helm

https://helm.sh/docs/intro/install

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

## Example 1: Nginx chart

`values.yaml`
```yaml
image:
  repository: nginx
  tag: latest
  pullPolicy: Always

```

`templates/deployment.yaml`
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ include "nginx-chart.fullname" . }}
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: {{ include "nginx-chart.fullname" . }}
  template:
    metadata:
      labels:
        app: {{ include "nginx-chart.fullname" . }}
    spec:
      containers:
        - name: {{ .Chart.Name }}
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
          imagePullPolicy: {{ .Values.image.pullPolicy }}
          ports:
            - containerPort: 80
          env:
            {{- range .Values.env }}
            - name: {{ .name }}
              value: {{ .value }}
            {{- end }}
```

```shell
helm install my-nginx nginx-chart
```
