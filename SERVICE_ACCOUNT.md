# Service Account

KEP-1205
* `TokenRequestAPI`
* TokenRequestAPI will create a token that is audience bound, time bound, and object bound
* A secret object is no longer mounted as a volume to a newly created object
* A token is mounted as a projected volume to a newly created object

KEP-2799
* Create a service account will not create a token
* A token has expiration date by default

```shell
kubectl get serviceaccount
```

create a service account and token

```shell
kubectl create serviceaccount dashboard
```

```shell
kubectl create token dashboard
```

```shell
kubectl describe serviceaccount dashboard
```

```shell
kubectl describe serviceaccount dashboard-token-kbbdm
```

use this token to authenticate into Kubernetes Dashbboard

