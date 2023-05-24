# SET UP

```shell
kubectl version --output=yaml
kubectl config view
kubectl config get-contexts
```

```shell
kubectl config view --minify | grep namespace:
kubectl config set-context --current --namespace=default
```

## GCP

https://cloud.google.com/kubernetes-engine/docs/how-to/cluster-access-for-kubectl

```shell
gcloud auth list
```

```shell
gcloud auth login
gcloud init
gcloud container clusters get-credentials CLUSTER_NAME \
    --region=CLUSTER_REGION
```

## AWS

https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

https://docs.aws.amazon.com/eks/latest/userguide/create-kubeconfig.html


```shell
aws configure
```

```shell
aws eks update-kubeconfig --region region-code --name my-cluster
```
