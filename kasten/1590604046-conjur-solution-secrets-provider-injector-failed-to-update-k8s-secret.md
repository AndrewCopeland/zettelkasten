# 1590604046 conjur-solution-secrets-provider-injector-failed-to-update-k8s-secret
#conjur #solution #secrets-provider #k8s

You must make sure that the service account that is attached to the `secrets-provider` container has the following RBAC permissions in k8s to be able to update a specific secret.

```yaml
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: < TEST_APP_SERVICE_ACCOUNT_NAME >
  namespace: < TEST_APP_NAMESPACE_NAME >

---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: secrets-access
rules:
  - apiGroups: [""]
    resources: ["secrets"]
    verbs: [ "get", "update" ]

---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  namespace: < TEST_APP_NAMESPACE_NAME >
  name: secrets-access-binding
subjects:
  - kind: ServiceAccount
    namespace: < TEST_APP_NAMESPACE_NAME >
    name: < TEST_APP_SERVICE_ACCOUNT_NAME >
roleRef:
  kind: ClusterRole
  apiGroup: rbac.authorization.k8s.io
  name: secrets-access
```


## Links
- [1590603996-conjur-error-secrets-provider-injector-failed-to-update-k8s-secret.md](1590603996-conjur-error-secrets-provider-injector-failed-to-update-k8s-secret.md)
