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

# granting host access to authenticate using authn-k8s
- !grant
  role: !group conjur/authn-k8s/<authenticator id>/apps
  member: !host app1
```

To authenticate an entire namespace load the following policy:
```yaml
- !host
  id: entire_namespace
  annotations:
    authn-k8s/namespace: <application namespace>
    authn-k8s/authentication-container-name: authenticator

# granting host access to authenticate using authn-k8s
- !grant
  role: !group conjur/authn-k8s/<authenticator id>/apps
  member: !host entire_namespace
```

Now that the hosts have been loaded into conjur now we need to deploy the sidecar/init container to our application.
[Example sidecar/init container manifest](1586878728-conjur-authn-k8s-side-car-manifest.md)


## Links
- [1586274452-conjur-authn-k8s.md](1586274452-conjur-authn-k8s.md)
- [1586875794-conjur-authn-k8s-external-authenticator.md](1586875794-conjur-authn-k8s-external-authenticator.md)
- [1586878728-conjur-authn-k8s-side-car-manifest.md](1586878728-conjur-authn-k8s-side-car-manifest.md)
