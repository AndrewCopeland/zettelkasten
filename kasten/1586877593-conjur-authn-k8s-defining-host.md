# 1586877593 conjur-authn-k8s-defining-host
#conjur #authn-k8s #host #policy

A conjur `authn-k8s` host identifies a specific k8s resource.
This host will represent a specific service accounts from within a namespace
```yaml
- !host
  id: app1
  annotations:
    authn-k8s/namespace: <application namespace>
    authn-k8s/service-account: <application service account>
    authn-k8s/authentication-container-name: authenticator
```

To authenticate an entire namespace load the following policy:
```yaml
- !host
  id: entire_namespace
  annotations:
    authn-k8s/namespace: <application namespace>
    authn-k8s/authentication-container-name: authenticator
```



## Links
- [1586875794-conjur-authn-k8s-external-authenticator.md](1586875794-conjur-authn-k8s-external-authenticator.md)
