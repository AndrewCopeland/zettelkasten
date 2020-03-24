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
```

## Use cases
- [Authenticating an AWS Fargate or ECS instance](1585068641-aws-fargate-iam-authn-conjur.md)
- [Authenticating an EC2 instance](https://github.com/AndrewCopeland/conjur-iam-api-key#ec2-usage)
- [Authenticating a Lambda function](https://github.com/AndrewCopeland/conjur-iam-api-key#lambda-usage)
- [Using summon with IAM role on EC2](https://github.com/AndrewCopeland/conjur-iam-api-key#summon-usage)

## Links
- [1585072449-conjur-home-authenticators.md](1585072449-conjur-home-authenticators.md)
- [1585068641-aws-fargate-iam-authn-conjur.md](1585068641-aws-fargate-iam-authn-conjur.md)
