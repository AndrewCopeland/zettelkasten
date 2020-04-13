# 1585094553 get-conjur-self-signed-certificate
#conjur #self-signed

Run this command to get the conjur self signed public certificate.

```bash
openssl s_client -showcerts -connect <conjur instance host name>:443 < /dev/null 2> /dev/null | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p'
```

## Links
- [1585094442-add-certificate-to-java-key-store.md](1585094442-add-certificate-to-java-key-store.md)
