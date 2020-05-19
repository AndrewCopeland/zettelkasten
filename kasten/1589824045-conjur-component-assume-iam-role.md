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
  - !variable iam-user-name
  - !variable iam-user-access-key
  - !variable iam-user-secret-access-key
  - !group

  # use the webservice '/assumeIamRole' endpoint
  - !permit
    role: !group
    resource: !webservice
    privileges: [ read, assumeIamRole ]

  # create a group that can only assume one iam role
  - !group 77394949373763/iam-role-name
  - !grant
    role: !group
    member: !group 77394949373763/iam-role-name
  - !permit
    role: !group 77394949373763/iam-role-name
    resource: !webservice
    privileges: [ 77394949373763/iam-role-name ]
```

- `!host assume-iam-role-component` will be the identity of the component
- `!webservice assume-iam-role` will represent the webservice
- `!variable iam-user-name` will represent the iam user    
- `!variable iam-user-access-key` will be the iam users access key
- `!variable iam-user-secret-access-key` will be the iam users secret access key
- The above variables will be used to assume specific IAM roles and those IAM role credentials will be returned

Configure an application to assume a specific IAM role
```yaml
- !host client

# give host ability to use the assume endpoint for a specific role
- !grant
  role: !group assume-iam-role/77394949373763/iam-role-name
  member: !host client
```

Now the `!host client` will the ability to request the conjur component to assume the iam role of `iam-role-name` for aws account `77394949373763` using the curl command below:
```bash
curl -H "<conjur access token>" https://conjur-assume-iam-role/assumeIamRole/77394949373763/iam-role-name
{
<AWS CREDENTIAL CONTENT>
}

```

## Links
- [1587138955-conjur-component-config.md](1587138955-conjur-component-config.md)
