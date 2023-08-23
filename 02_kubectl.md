# Configurations

## Client

https://kubernetes.io/docs/reference/kubectl/cheatsheet/#kubectl-autocomplete

```shell
kubectl version
```

```shell
apt install bash-completion 
source <(kubectl completion bash)
echo "source <(kubectl completion bash)" >> ~/.bashrc
```

```shell
source <(kubectl completion zsh)
echo '[[ $commands[kubectl] ]] && source <(kubectl completion zsh)' >> ~/.zshrc
```

```shell
alias k=kubectl
complete -o default -F __start_kubectl k
```

## Cluster

https://kubernetes.io/docs/reference/setup-tools/kubeadm/implementation-details

```shell
ls /etc/kubernetes/manifests
```

```shell
ls /etc/kubernetes
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

See cluster information
```shell
kubectl config get-contexts
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

See cluster information
```shell
kubectl config get-contexts
```
