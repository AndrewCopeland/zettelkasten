# 1585772220 conjur-authn-k8s-implementation
#conjur #authn-k8s

The following policy will beed to be loaded to configure conjur to use the authn-k8s authenticatior.

## Loading policy for apps => followers
```yml
- !policy
  id: conjur/authn-k8s/<authenticator name>
  body:
  - !variable ca/key
  - !variable ca/cert
  - !webservice
  - !groups apps
  - !permit
    role: !group apps
    resource: !webservice
    privileges: [ read, authenticate ]
```

## Loading policy for followers => masters
Variables prefixed with `kubernetes/*` are only required when
running outside of Kubernetes. Variables prefixed with `ca/*` 
are always required.

```yml
- !policy
  id: conjur/authn-k8s/<authenticator name>
  body:
  # The k8s cluster api url.
  - !variable kubernetes/api-url
  # The k8s cluster cert.
  - !variable kubernetes/ca-cert
  # The k8s service token used for conjur to connect to the k8s cluster.
  - !variable kubernetes/service-account-token

  # The ca private key
  - !variable ca/key
  # the ca certificate
  - !variable ca/cert
  
  # This webservice represents the k8s authenticator
  - !webservice
  # Any member of this group will be able to use this k8s authenticator
  - !group apps
  - !permit:
    role: !group apps
    resource: !webservice
    privieleges: [ authenticate ]

  # This hosts is needed for followers running in k8s to access the seed fetcher service
  - !host
    id: followers
    annotations:
      authn-k8s/namespace: <dap follower namespace>
      authn-k8s/service-account: conjur-cluster
      authn-k8s/authentication-container-name: authenticator
  - !grant
    role: !group apps
    member: !host followers
```




## Links
