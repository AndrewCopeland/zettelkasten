# 1585160674 bash-openssl
#bash #openssl

How to examine a certificate
```bash
openssl x509 -noout -text -in file.pem
```

How to verify a root bundle with certificare:
```bash
openssl verify -CAfile root-bundle.pem file.pem
```

Convert .pfx to a .pem:
```bash
openssl pkcs12 -in <pkcs-12-certificate-and-key-file> -out <pem-certificate-and-key-file> 
```

Remove private key password:
```bash
openssl rsa -in [file1.key] -out [file2.key]
```


# Links
- [1584724813-bash-cheat-sheet.md](1584724813-bash-cheat-sheet.md)
