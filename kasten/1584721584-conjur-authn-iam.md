# 1584721584 conjur-authn-iam
#conjur #authn

The conjur `authn-iam` authenticator allows the ability to authenticate using an AWS IAM role.


## Advantages
- No secret zero
- Use AWS identities to retrieve secrets from conjur
- IAM roles are widely accepted and understood

## Disadvatages
- Creation of the signed request requires code within the application

## Configuration
```yaml
- !policy
  id: conjur/authn-iam/<service id>
  body:
  - !webservice
  - !group apps
  - !permit
    role: !group apps
    resource: !webservice
    privileges: [ read, authenticate ]

# an example of a host that will authenticate using an IAM role
- !host team1/<aws account number>/<iam role name>
- !grant
  role: !group conjur/authn-iam/<service id>/apps
  member: !host team1/<aws account number>/<iam role name>
```

Once the policy has been loaded we must enable the authenticator by execing into the conjur appliance and performing the following:
```bash
evoke variables set CONJUR_AUTHENTICATORS=authn,authn-iam/<service id>
```

## Use cases
- [Authenticating an AWS Fargate or ECS instance](1585068641-aws-fargate-iam-authn-conjur.md)
- [Authenticating an EC2 instance](https://github.com/AndrewCopeland/conjur-iam-api-key#ec2-usage)
- [Authenticating a Lambda function](https://github.com/AndrewCopeland/conjur-iam-api-key#lambda-usage)
- [Using summon with IAM role on EC2](https://github.com/AndrewCopeland/conjur-iam-api-key#summon-usage)

## Links
- [1585072449-conjur-home-authenticators.md](1585072449-conjur-home-authenticators.md)
- [1585068641-aws-fargate-iam-authn-conjur.md](1585068641-aws-fargate-iam-authn-conjur.md)
- [1585328995-ec2-metadata-url-token.md](1585328995-ec2-metadata-url-token.md)
- [1585603925-lambda-access-key-environment-vars.md](1585603925-lambda-access-key-environment-vars.md)
- [1586469583-aws-eks-iam-roles-with-service-accounts.md](1586469583-aws-eks-iam-roles-with-service-accounts.md)
