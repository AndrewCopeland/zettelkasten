# 1588967540 conjur-authn-k8s-manually-generating-certificates
#conjur #authn-k8s

To manually get the conjur ca key and ca cert for `authn-k8s` in conjur perform the following bash script:
```bash
#!/bin/bash
set -e
AUTHENTICATOR_ID='lab'
CONJUR_ACCOUNT='lab'
# Generate OpenSSL private key
openssl genrsa -out ca.key 2048
CONFIG="
[ req ]
distinguished_name = dn
x509_extensions = v3_ca
[ dn ]
[ v3_ca ]
basicConstraints = critical,CA:TRUE
subjectKeyIdentifier   = hash
authorityKeyIdentifier = keyid:always,issuer:always
"
# Generate root CA certificate
openssl req -x509 -new -nodes -key ca.key -sha1 -days 3650 -set_serial 0x0 -out ca.cert \
  -subj "/CN=conjur.authn-k8s.$AUTHENTICATOR_ID/OU=Conjur Kubernetes CA/O=$CONJUR_ACCOUNT" \
  -config <(echo "$CONFIG")
# Verify cert
openssl x509 -in ca.cert -text -noout
# Load variable values
conjur variable values add conjur/authn-k8s/$AUTHENTICATOR_ID/ca/key "$(cat ca.key)"
conjur variable values add conjur/authn-k8s/$AUTHENTICATOR_ID/ca/cert "$(cat ca.cert)"
```



## Links
- [1588967425-conjur-enable-authn-k8s-required-key-not-available.md](1588967425-conjur-enable-authn-k8s-required-key-not-available.md)
