# Service Account

https://kubernetes.io/docs/concepts/security/service-accounts

* A service account is a type of non-human account
* Kubernetes automatically creates `default` service accounts for every namespace in your cluster
* If RABC is enabled, the `default` service accounts in each namespace get no permissions other than the default API discovery permissions

## KEP-1205

https://github.com/kubernetes/enhancements/tree/master/keps/sig-auth/1205-bound-service-account-tokens

* Implement `TokenRequestAPI` in the core `apiserver`
* A `secret` is no longer mounted as a volume to a newly created object
* A `service_account_token` is mounted as a projected volume to a newly created object
* A `service_account_token` is audience bound, time bound, and object bound

## KEP-2799

https://github.com/kubernetes/enhancements/tree/master/keps/sig-auth/2799-reduction-of-secret-based-service-account-token

* Encourage users to use `TokenRequestAPI` or manually-created secret-based service account tokens
* Create a service account will not create a `service_account_token`

## Demo

https://www.linkedin.com/pulse/service-account-token-changes-kubernetes-version-124-shafeeque-aslam

Create a service account without secret or token
```shell
kubectl create serviceaccount dashboard
```

Create time-bound (1h) tokens using `TokenRequestAPI`
```shell
kubectl create token dashboard
```

Expect: no secret
```
kubectl get secret
```
