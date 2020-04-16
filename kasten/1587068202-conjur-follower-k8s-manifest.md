# 1587068202 conjur-follower-k8s-manifest
#conjur #follower #k8 #manifest


```yaml
---
apiVersion: v1
kind: Namespace
metadata:
  name: dap

---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: conjur-cluster
  namespace: dap

---
apiVersion: v1
kind: ConfigMap
metadata:
  name: k8s-app-ssl
  namespace: dap
data:
  ssl-certificate: |
    -----BEGIN CERTIFICATE-----
    MIIDZTCCAk2gAwIBAgIVAPhr0LK0JLaZ3HDwwEmgDcCaZK23MA0GCSqGSIb3DQEB
    CwUAME4xETAPBgNVBAoMCGN5YmVyYXJrMRIwEAYDVQQLDAlDb25qdXIgQ0ExJTAj
    BgNVBAMMHGRhcG1hc3Rlci5jeWJlcmFya2RlbW8ubG9jYWwwHhcNMjAwMTI3MTU0
    MzU0WhcNMzAwMTI0MTU0MzU0WjAlMSMwIQYDVQQDDBpkYXBtYXN0ZXIuY3liZXJh
    cmtkZW1vLmNvbTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAOsU/daX
    p5N0+0Cm00EEk/Q0BLDYZvbAJxWL0JrVwYrH9IYputv7BZumjaY2g9guEdMhngIL
    yXblkceDqa2csGD5HGGYfkDivh3kqbNM5D9WP/mirKgOmkmzqb2Y16dWUsnrvqPZ
    I2dyAm81wuCGieXZBzZG4kAtUDjQkC9cdyb2ocnRC4GWrqWHvEUhNrXTqTBpOr45
    CTl3YQ1rsbJsjLIxOfn0zOoEC3+irVLERFqCE1TU3KhJWbWDdGKs1rx2X8YOaIEB
    1uJxWrSWvO8PhSJXAuMyXDO89FELfNtPnch68VgVnTVJmvrMPw/ZZfuz2eKs8HAT
    yilzqSjY5IedNPsCAwEAAaNjMGEwDgYDVR0PAQH/BAQDAgWgMB0GA1UdDgQWBBTa
    OaPuXmtLDTJVv++VYBiQr9gHCTAwBgNVHREEKTAnghpkYXBtYXN0ZXIuY3liZXJh
    cmtkZW1vLmNvbYIJZGFwbWFzdGVyMA0GCSqGSIb3DQEBCwUAA4IBAQBW45PBijSS
    R3LTgRRu7GDVcBf45MaifqLIRxKkbiIaVbnH8gD2ydfVfSAH3GGcuDwZgdpAZQEr
    Y0ctmsmZNofNM/Kh+9bjyPAlk5kYBn7oIwptcROJKFtSppOIe3m27ruEMjSlpcaB
    qlGo/6G5k7g3rT3olRYtRYZBZo3eTIFwcOP/jh2ZfrljIMM3tM3MDwcab7F1qH23
    kej9+6zwq+P26Fx9d4ukutKIL2G6IGt7aw+oWgh8hHrqfciqs4wthQLOD4EE0v5C
    AuD5AY7DUByluHTmELYB0d+aTxCkjP8u2VgmOr+N0TNpb298GsGHVo8p2Q5ZyjV4
    0WIRrFELdYxT
    -----END CERTIFICATE-----
    -----BEGIN CERTIFICATE-----
    MIIDxTCCAq2gAwIBAgIUSt65MCyd7RbPU+8Ou4XHfRR7JIowDQYJKoZIhvcNAQEL
    BQAwTjERMA8GA1UECgwIY3liZXJhcmsxEjAQBgNVBAsMCUNvbmp1ciBDQTElMCMG
    A1UEAwwcZGFwbWFzdGVyLmN5YmVyYXJrZGVtby5sb2NhbDAeFw0xOTEyMTgxOTM4
    MDdaFw0yOTEyMTUxOTM4MDdaME4xETAPBgNVBAoMCGN5YmVyYXJrMRIwEAYDVQQL
    DAlDb25qdXIgQ0ExJTAjBgNVBAMMHGRhcG1hc3Rlci5jeWJlcmFya2RlbW8ubG9j
    YWwwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDBguU8KMwf9YI9QOXO
    hMA5iRVd+y9oon2bZb0Q51o6FkgjBTWdznDnvzOAtJI9Oue0luzGD/eXZwlO9klm
    gfC21E2PyPaZu1aqFXkHVnssWr4+Qxe2Ks/tSp4KAHjUN3EuMnjEpxIuOruO1y/j
    Pzdu54ZIu+ipNIU90eNPW9rKkEOaFVLq0GbaMDOPmTHFfExtUqj3kbC9AABk98g6
    1P1E9WljPgu6XBW90ov/luEnYHPJUKNrROyc0DotypBQ/AZ1ElEqkClB34ieC0UT
    6NkfsCTbWcpkWnYg5JMLv7UP4Vu9IuABMr4UD5SaX0iwmT8dVMo95LlTTsoZViGE
    LuBFAgMBAAGjgZowgZcwOgYDVR0RBDMwMYIcZGFwbWFzdGVyLmN5YmVyYXJrZGVt
    by5sb2NhbIIJbG9jYWxob3N0ggZjb25qdXIwHQYDVR0OBBYEFNLUmh/SIm6fMw1x
    nubPItjFdOv6MB8GA1UdIwQYMBaAFNLUmh/SIm6fMw1xnubPItjFdOv6MAwGA1Ud
    EwQFMAMBAf8wCwYDVR0PBAQDAgHmMA0GCSqGSIb3DQEBCwUAA4IBAQCi+5Ir5z4u
    g6EObZdqwdpNPZx3yGIMBA6Vq0di/efXfW+ti2hbiDPa/V3tHgcaO0dmuhULo80W
    QwUQ/QFlJdESoTqyyJ/baRj+JPqjD0igkqOHfLhzUlmvAR1bdCP8VALGrAhJWY74
    AUZGew8W63NS11vDp1TJ1Q8B+GSmzhkcO8kM/0Hdqs0xBGB+ltZnYaX7ik9Z5SCe
    dMfDSVbFRuaAFSYhpFAFQINPtLm+rgiQSVioRio9hk3hx1jfCSe+ZUhvF6uW43Sa
    +5gJxcYEC1HUZoHssizOUQ4U+hv0icJRcCaXJRY3ssT3YOU8zu+k7A+gMJmQ/2Cx
    v4GuEPN1Wbi8
    -----END CERTIFICATE-----

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
  namespace: dap
roleRef:
  kind: ClusterRole
  name: conjur-authenticator
  apiGroup: rbac.authorization.k8s.io

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: dap-access-node
  namespace: dap
  labels:
    app: cyberark
spec:
  replicas: 1
  selector:
    matchLabels:
      role: access
  template:
    metadata:
      labels:
        role: access
    spec:
      serviceAccountName: conjur-cluster
      volumes:
      - name: seedfile
        emptyDir:
          medium: Memory
      - name: conjur-token
        emptyDir:
          medium: Memory
      initContainers:
      - name: authenticator
        image: cyberark/dap-seedfetcher:latest
        imagePullPolicy: IfNotPresent
        env:
          - name: CONJUR_SEED_FILE_URL
            value: https://dapmaster.cyberarkdemo.com/configuration/cyberark/seed/follower
          - name: SEEDFILE_DIR
            value: /tmp/seedfile
          - name: FOLLOWER_HOSTNAME
            value: access
          - name: AUTHENTICATOR_ID
            value: k8s-follower
          - name: CONJUR_ACCOUNT
            value: cyberark
          - name: CONJUR_SSL_CERTIFICATE
            valueFrom:
              configMapKeyRef:
                name: k8s-app-ssl
                key: ssl-certificate
          - name: MY_POD_NAME
            valueFrom:
              fieldRef:
                fieldPath: metadata.name
          - name: MY_POD_NAMESPACE
            valueFrom:
              fieldRef:
                fieldPath: metadata.namespace
          - name: MY_POD_IP
            valueFrom:
              fieldRef:
                fieldPath: status.podIP
          - name: CONJUR_AUTHN_LOGIN
            value: "host/conjur/authn-k8s/k8s-follower/apps/dap/service_account/conjur-cluster"
        volumeMounts:
          - name: seedfile
            mountPath: /tmp/seedfile
          - name: conjur-token
            mountPath: /run/conjur
      containers:
      - name: node
        imagePullPolicy: IfNotPresent
        image: captainfluffytoes/dap:10.10
        command: ["/tmp/seedfile/start-follower.sh"]
        env:
          - name: CONJUR_AUTHENTICATORS
            value: "authn-k8s/k8s-follower"
          - name: SEEDFILE_DIR
            value: /tmp/seedfile
        ports:
          - containerPort: 443
            protocol: TCP
          - containerPort: 5432
            protocol: TCP
          - containerPort: 1999
            protocol: TCP
        readinessProbe:
          httpGet:
            path: /health
            port: 443
            scheme: HTTPS
          initialDelaySeconds: 15
          timeoutSeconds: 5
        volumeMounts:
          - name: seedfile
            mountPath: /tmp/seedfile
            readOnly: true
        resources:
          requests:
            memory: "1Gi"
            cpu: "250m"
          limits:
            memory: "2Gi"
            cpu: "500m"
---
apiVersion: v1
kind: Service
metadata:
  name: access
  namespace: dap
spec:
  ports:
  - name: https
    port: 443
    targetPort: 443
  selector:
    role: access
  type: ClusterIP
```



## Links
