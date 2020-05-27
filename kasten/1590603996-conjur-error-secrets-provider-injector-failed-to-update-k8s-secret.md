# 1590603996 conjur-error-secrets-provider-injector-failed-to-update-k8s-secret
#conjur #error #injector #secrets-provder #k8s

Thiss relates to using the conjur cyberark secrets provider for k8s. The docker image can be found [here](https://hub.docker.com/r/cyberark/secrets-provider-for-k8s/tags)

```
ERROR: 2020/05/27 17:01:33 k8s_secrets_client.go:46: CSPFK022E Failed to update k8s secret
ERROR: 2020/05/27 17:01:33 provide_conjur_secrets.go:165: CSPFK022E Failed to update k8s secret
ERROR: 2020/05/27 17:01:33 provide_conjur_secrets.go:88: CSPFK023E Failed to update K8s secrets
```

## Solutions
- [Update the k8s RBAC so service account can update a k8s secret](1590604046-conjur-solution-secrets-provider-injector-failed-to-update-k8s-secret.md)

## Links
- [1590604046-conjur-solution-secrets-provider-injector-failed-to-update-k8s-secret.md](1590604046-conjur-solution-secrets-provider-injector-failed-to-update-k8s-secret.md)
