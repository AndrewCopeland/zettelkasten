# 1588967425 conjur-enable-authn-k8s-required-key-not-available
#conjur #authn-k8s #configure #error

When configuring the `authn-k8s` we must set a ca key and certificate using this command:
```bash
chpst -u conjur conjur-plugin-service possum rake authn_k8s:ca_init["conjur/authn-k8s/non-prod-a"]
```

However when executing this command I am receieveing the following error:
```bash
root@dc8fd2af793e:/# chpst -u conjur conjur-plugin-service possum rake authn_k8s:ca_init["conjur/authn-k8s/non-prod-a"]
Searching conjur keyring for key possum
keyctl_search: Required key not available
```

### Solution
1. [Generate certificate and load secrets manually](1588967540-conjur-authn-k8s-manually-generating-certificates.md)


## Links
- [1588967540-conjur-authn-k8s-manually-generating-certificates.md](1588967540-conjur-authn-k8s-manually-generating-certificates.md)
