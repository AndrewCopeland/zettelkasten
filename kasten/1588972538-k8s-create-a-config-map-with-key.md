# 1588972538 k8s-create-a-config-map-with-key
#k8s #configmap

Create a config map object with one line of kubectl.
```bash
kubectl -n <namespace> create configmap k8s-app-ssl --from-file=ssl-certificate=<path to pem>
```

## Links
