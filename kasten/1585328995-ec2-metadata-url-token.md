# 1585328995 ec2-metadata-url-token
#aws #authn-iam

To obtain the IAM role associated with this EC2 instance perform the following:
```bash
curl http://169.254.169.254/latest/meta-data/iam/security-credentials/
```

To obtain the access key, secret access key and token for this iam user:
```bash
curl http://169.254.169.254/latest/meta-data/iam/security-credentials/<iam role name>
```

## Links
- [1584721584-conjur-authn-iam.md](1584721584-conjur-authn-iam.md)
- [1586469583-aws-eks-iam-roles-with-service-accounts.md](1586469583-aws-eks-iam-roles-with-service-accounts.md)
