# 1589824045 conjur-component-assume-iam-role
#conjur #component #assume #iam #role

This could be used to assume an iam role using a conjur access token.
This could allow on-prem/in-cloud identities to authenticate to conjur and then assume an IAM role.
```
Azure -> AWS S3 Bucket
GCP -> AWS Lambda function
On-prem api key -> AWS s3 bucket
On-prem k8s pod -> AWS Lambda function
```


Configure the `assume-iam-role` webservice:
```yaml
- !host assume-iam-role-component
- !policy
  id: assume-iam-role
  owner: !host assume-iam-role-component
  body:
  - !webservice
  - !group
  - !permit
    role: !group
    resource: !webservice
    privileges: [ read, assume ]
  - !variable iam-user-name
  - !variable iam-user-access-key
  - !variable iam-user-secret-access-key
```

- `!host assume-iam-role-component` will be the identity of the component
- `!webservice assume-iam-role` will repersent the webservice
- `!variable iam-user-name` will repersent the iam user    
- `!variable iam-user-access-key` will be the iam users access key
- `!variable iam-user-secret-access-key` will be the iam users secret access key
- The above variables will be used to assume specific IAM roles and those IAM role credentials will be returned

Configure an application to assume a specific IAM role
```yaml
- !host app1

# give host ability to use the assume endpoint
- !grant
  role: !group assume-iam-role
  member: !host app1

# give host ability to assume a specific iam role 's3-bucket-iam-role-name'
- !permit
  role: !host app1
  resource: !webservice assume-iam-role
  privileges: [ s3-bucket-iam-role-name ]
```

Now the `!host app1` will the ability to hit the conjur component to assume the iam role of `s3-bucket-iam-role-name` using the curl command below:
```bash
curl -H "<conjur access token>" -X POST --data '{"assume-iam-role": "s3-bucket-iam-role-name"}' https://conjur-assume-iam-role/assume
{
<AWS CREDENTIAL CONTENT>
}

```

## Links
- [1587138955-conjur-component-config.md](1587138955-conjur-component-config.md)
