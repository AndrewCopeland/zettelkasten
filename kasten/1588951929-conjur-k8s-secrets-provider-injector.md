# 1588951929 conjur-k8s-secrets-provider-injector
#conjur #authn-k8s #injector #k8s-provider

Create a host as you normally would with the `authn-k8s` authenticator.
Also this host should have access to the secrets we are injecting into k8s secrets.

Add a `conjur-map` data entry to specify the mapping between the k8s secret data key and the secret in DAP.

```yaml
# authn-k8s-<authenticator_id>-secrets.yml
---
apiVersion: v1
kind: Secret
metadata:
  name: db-credentials
type: Opaque
data:
  DBName:   bXlhcHBEQg==
stringData:
  conjur-map: |-   
    username: db_credentials/username   
    password: db_credentials/password
```

Load this manifest
```bash
kubectl apply -f authn-k8s-<authenticator_id>-secrets.yml
```

Configure k8s RBAC permissions by loading the manifect:
```yaml
# conjur-role.yml
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: < TEST_APP_NAMESPACE_NAME >

---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: secrets-access
rules:
  - apiGroups: [""]
    resources: ["secrets"]
    verbs: [ "get", "patch" ]

---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  namespace: < TEST_APP_NAMESPACE_NAME >
  name: secrets-access-binding
subjects:
  - kind: ServiceAccount
    namespace: < TEST_APP_NAMESPACE_NAME >
    name: < TEST_APP_NAMESPACE_NAME >
roleRef:
  kind: ClusterRole
  apiGroup: rbac.authorization.k8s.io
  name: secrets-access
```

apply the above manifest
```bash
kubectl apply -f conjur-role.yml
```


Example deployment of the application should look like the following:

```yaml
---
apiVersion: extensions/v1
kind: Deployment
metadata:
  labels:
    app: test-app
  name: test-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: test-app
  template:
    metadata:
      labels:
        app: test-app
    spec:
      serviceAccountName: < TEST_APP_NAMESPACE_NAME  >
      containers:
        - image: < TEST_APP_DOCKER_IMAGE >
          imagePullPolicy: Always
          name: test-app
          env:
            - name: DB_USERNAME
              valueFrom:
                secretKeyRef:
                  name: db-credentials
                  key: username
            - name: DB_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: db-credentials
                  key: password
      initContainers:
        - image: 'cyberark/secrets-provider-for-k8s'
          imagePullPolicy: Always
          name: cyberark-secrets-provider-for-k8s
          env:
            - name: CONTAINER_MODE
              value: init

            - name: MY_POD_NAME
              valueFrom:
                fieldRef:
                  apiVersion: v1
                  fieldPath: metadata.name

            - name: MY_POD_NAMESPACE
              valueFrom:
                fieldRef:
                  apiVersion: v1
                  fieldPath: metadata.namespace

            - name: MY_POD_IP
              valueFrom:
                fieldRef:
                  fieldPath: status.podIP

            - name: CONJUR_VERSION
              value: '5'

            - name: CONJUR_APPLIANCE_URL
              value: >-
                < CONJUR_APPLIANCE_URL >

            - name: CONJUR_AUTHN_URL
              value: >-
                < CONJUR_AUTHN_URL >

            - name: CONJUR_ACCOUNT
              value: < CONJUR_ACCOUNT >

            - name: CONJUR_AUTHN_LOGIN
              value: >-
                host/conjur/authn-k8s/<authenticator-id>/apps/namespace-based-app


            - name: CONJUR_SSL_CERTIFICATE
              valueFrom:
                configMapKeyRef:
                  name: < CONFIG_MAP_NAME >
                  key: ssl-certificate

            - name: K8S_SECRETS
              value: db-credentials

            - name: SECRETS_DESTINATION
              value: k8s_secrets
```


## Links
