# 1586878728 conjur-authn-k8s-side-car-manifest
#conjur #authn-k8s #manifest #sidecar

Add the following to the containers spec of the Deployment manifest:
```yaml
      - name: authenticator
        image: cyberark/conjur-authn-k8s-client
        imagePullPolicy: IfNotPresent
        env:
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
          - name: CONJUR_APPLIANCE_URL
            value: <conjur appliance url>
          - name: CONJUR_AUTHN_URL
            value: <conjur appliance url>/authn-k8s/<authenticator id>
          - name: CONJUR_ACCOUNT
            value: <conjur account>
          - name: CONJUR_AUTHN_LOGIN
            value: <conjur host e.g. host/app1>
          - name: CONJUR_SSL_CERTIFICATE
            valueFrom:
              configMapKeyRef:
                name: k8s-app-ssl
                key: ssl-certificate
        volumeMounts:
          - mountPath: /run/conjur
            name: conjur-access-token
      volumes:
        - name: conjur-access-token
          emptyDir:
            medium: Memory
```



## Links
- [1586274452-conjur-authn-k8s.md](1586274452-conjur-authn-k8s.md)
- [1586877593-conjur-authn-k8s-defining-host.md](1586877593-conjur-authn-k8s-defining-host.md)
