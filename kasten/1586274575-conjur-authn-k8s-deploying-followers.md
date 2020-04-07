# 1586274575 conjur-authn-k8s-deploying-followers
#conjur #k8s #followers #authn-k8s

We must configure the master to support external k8s authentication so we can auto provision and scale our conjur followers from within the k8s ckuster:

The following policy will need to be loaded to allow external authentication:
```yaml
### kubernetes-followers.yml
- !policy
  id: conjur/authn-k8s/<authenticator name>
  body:
  # Runtime configuration variables required by the authenticator.
  # Variables prefixed with `kubernetes/*` are only required when
  # running outside of Kubernetes. Variables prefixed with `ca/*`
  # are always required.
  - !variable kubernetes/service-account-token
  - !variable kubernetes/ca-cert
  - !variable kubernetes/api-url
  - !variable ca/key
  - !variable ca/cert
  # This webservice represents the K8s authenticator
  - !webservice
  # The `apps` policy defines K8s resources that 
  # can be authenticated.
  - !policy
    id: apps
    body:
    # All application roles that are run in K8s must have
    # membership in the `apps` layer
    - !layer
    # `authenticated-resources` is an array of hosts that map to
    # resources in K8s. The naming convention is
    # namespace/resource type/resource name
    - &authenticated-resources
      - !host
        id: test-app
        annotations:
          authn-k8s/namespace: <dap follower namespace>
          authn-k8s/service-account: conjur-cluster
          authn-k8s/authentication-container-name: authenticator
    
    - !grant
      role: !layer
      members: *authenticated-resources
  # Members of `apps` are allowed to authenticate
  - !permit
    role: !layer apps
    privilege: [ authenticate ]
    resource: !webservice
# =================================================
# == Register the Seed Service
# =================================================
- !policy
  id: conjur/seed-generation
  body:
  # This webservice represents the Seed service API
  - !webservice
  # Hosts that generate seeds become members of the
  # `consumers` layer.
  - !layer consumers
  # Authorize `consumers` to request seeds
  - !permit
    role: !layer consumers
    privilege: [ "execute" ]
    resource: !webservice
# =================================================
# == Grant entitlements
# =================================================
# Give followers permission to consume seeds
- !grant
  role: !layer conjur/seed-generation/consumers
  member: !host conjur/authn-k8s/<authenticator name>/apps/test-app
```

The `conjur/authn-k8s/<service id>` contains 3 extra variables when using k8s externally.
Load the policy above with the following command:
```bash
conjur policy load root kubernetes-followers.yml
```

Next is to load the k8s manifests that will repersent the conjur follower:
```yaml
# conjur-role.yml
---
apiVersion: v1
kind: Namespace
metadata:
  name: <dap follower namespace>
  labels:
    name: <dap follower namespace>
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: conjur-cluster
  namespace: <dap follower namespace>
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
  namespace: <dap follower namespace>
roleRef:
  kind: ClusterRole
  name: conjur-authenticator
  apiGroup: rbac.authorization.k8s.io
```

Load the manifest above with the following command:
```bash
kubectl apply -f conjur-role.yml
```

Now we must populate the variables values defined in the `conjur/authn-k8s/<service id>` policy:
```bash
# obtain the service account token secret
TOKEN_SECRET_NAME="$(kubectl get secrets -n <dap follower namespace> \
    | grep 'conjur.*service-account-token' \
    | head -n1 \
    | awk '{print $1}')"

# obtain the k8s CA cert
CA_CERT="$(kubectl get secret -n <dap follower namespace> $TOKEN_SECRET_NAME -o json \
    | jq -r '.data["ca.crt"]' \
    | base64 --decode)"

# obtain the service account token
SERVICE_ACCOUNT_TOKEN="$(kubectl get secret -n <dap follower namespace> $TOKEN_SECRET_NAME -o json \
    | jq -r .data.token \
    | base64 --decode)"

# obtain the k8s api url
API_URL="$(kubectl config view --minify -o json \
    | jq -r '.clusters[0].cluster.server')"

# now load the variables obtained from the steps above
conjur variable values add conjur/authn-k8s/<authenticator name>/kubernetes/ca-cert "$CA_CERT"
conjur variable values add conjur/authn-k8s/<authenticator name>/kubernetes/service-account-token "$SERVICE_ACCOUNT_TOKEN"
conjur variable values add conjur/authn-k8s/<authenticator name>/kubernetes/api-url "$API_URL"
```

On the master container we have to initialize and enable the authn-k8s/<authenticator name>:
```bash
docker exec <dap master container name> \
    chpst -u conjur conjur-plugin-service possum \
    rake authn_k8s:ca_init["conjur/authn-k8s/<authenticator name>"]

docker exec <dap master continaer name> \
    evoke variable set CONJUR_AUTHENTICATORS "authn,authn-k8s/<authenticator name>"
```

No everthing on the conjur side is configured its time ot focus on the k8s deployment. We have a git repo to help with this process:

```bash
git clone https://github.com/cyberark/kubernetes-conjur-deploy.git
```

Open the bootstrap.env file and modify the values to correspond with your specific k8s cluster, it should look something like:
```bash
export DOCKER_REGISTRY_PATH=openshift.myorg.com
export CONJUR_VERSION=5
export CONJUR_APPLIANCE_IMAGE=cyberark/conjur-appliance:10.9
export CONJUR_FOLLOWER_COUNT=2
export CONJUR_NAMESPACE_NAME=dap-follower
export PLATFORM=kubernetes
export OSHIFT_CLUSTER_ADMIN_USERNAME=cluster-admin
export OSHIFT_CONJUR_ADMIN_USERNAME=cluster-admin
export AUTHENTICATOR_ID=staging
export CONJUR_APPLIANCE_URL=https://conjur.myorg.com
export CONJUR_ACCOUNT=myorg
export FOLLOWER_SEED="$CONJUR_APPLIANCE_URL/configuration/$CONJUR_ACCOUNT/seed/follower"
export STOP_RUNNING_ENV="false"
```
We can start the deployment by running:
```bash
./start
```

## Links
- [1586274452-conjur-authn-k8s.md](1586274452-conjur-authn-k8s.md)
