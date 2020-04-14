# 1586875794 conjur-authn-k8s-external-authenticator
#conjur #authn-k8s #external

When a follower is deployed externally from the k8s cluster the authn-k8s authenticator needs 3 more variables since it needs to connect to the k8s cluster and validate pods attempting to authenticate.

Below is the policy that needs to be loaded to configure the external authn-k8s authenticator:
```yaml
# root.yml
- !policy
  id: conjur/authn-k8s/k8s-cluster-1
  body:
  - !variable kubernetes/service-account-token
  - !variable kubernetes/ca-cert
  - !variable kubernetes/api-url
  - !variable ca/key
  - !variable ca/cert

  - !webservice
  - !group apps
  - !permit
    role: !group apps
    resource: !webservice
    privileges: [ read, authenticate ]
```
Load the policy by executing the command:
`conjur policy load root root.yml`

Now that the authenticator is configured lets enable the authenticator by executing the following commands on the conjur follower container:
```bash
docker exec <dap follower container name> \
    chpst -u conjur conjur-plugin-service possum \
    rake authn_k8s:ca_init["conjur/authn-k8s/k8s-cluster-1"]
evoke variable set CONJUR_AUTHENTICATORS authn,authn-k8s/k8s-cluster-1
```

Next is to create a k8s service account that has the needed permissions for the `authn-k8s` to authenticate pods:
```yaml
# authn-k8s-service-account.yml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: conjur-cluster
  namespace: default
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: conjur-authenticator
rules:
- apiGroups: [""]
  resources: ["pods", "serviceaccounts"]
  verbs: ["get", "list"]
- apiGroups: ["extensions"]
  resources: [ "deployments", "replicasets"]
  verbs: ["get", "list"]
- apiGroups: ["apps"]
  resources: [ "deployments", "statefulsets", "replicasets"]
  verbs: ["get", "list"]
- apiGroups: [""]
  resources: ["pods/exec"]
  verbs: ["create", "get"]
---
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: conjur-authenticator
subjects:
- kind: ServiceAccount
  name: conjur-cluster
  namespace: default
roleRef:
  kind: ClusterRole
  name: conjur-authenticator
  apiGroup: rbac.authorization.k8s.io
```

Load the manifest by running:
```bash
kubectl create -f authn-k8s-service-account.yml
```

Now that we have created the service accounts, lets retrieve the needed variables using the `kubectl` command:
```bash
# obtain the service account token secret
TOKEN_SECRET_NAME="$(kubectl get secrets -n default\
    | grep 'conjur.*service-account-token' \
    | head -n1 \
    | awk '{print $1}')"

# obtain the k8s CA cert
CA_CERT="$(kubectl get secret -n default $TOKEN_SECRET_NAME -o json \
    | jq -r '.data["ca.crt"]' \
    | base64 --decode)"

# obtain the service account token
SERVICE_ACCOUNT_TOKEN="$(kubectl get secret -n default $TOKEN_SECRET_NAME -o json \
    | jq -r .data.token \
    | base64 --decode)"

# obtain the k8s api url
API_URL="$(kubectl config view --minify -o json \
    | jq -r '.clusters[0].cluster.server')"

# now load the variables obtained from the steps above
conjur variable values add conjur/authn-k8s/k8s-cluster-1/kubernetes/ca-cert "$CA_CERT"
conjur variable values add conjur/authn-k8s/k8s-cluster-1/kubernetes/service-account-token "$SERVICE_ACCOUNT_TOKEN"
conjur variable values add conjur/authn-k8s/k8s-cluster-1/kubernetes/api-url "$API_URL"
```

The k8s authenticator has been configured and enabled for our follower.
Next step is to define a `host` within conjur that represents a k8s resources.
[Creating hosts in conjur for authn-k8s](1586877593-conjur-authn-k8s-defining-host.md)


## Links
- [1586274452-conjur-authn-k8s.md](1586274452-conjur-authn-k8s.md)
- [1586877593-conjur-authn-k8s-defining-host.md](1586877593-conjur-authn-k8s-defining-host.md)
