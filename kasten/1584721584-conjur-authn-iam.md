# 1584721584 conjur-authn-iam
#conjur #authn

The conjur `authn-iam` authenticator allows the ability to authenticate using an AWS IAM role.

## Advantages
- No secret zero
- Use AWS identities to retrieve secrets from conjur
- IAM roles are widely accepted and understood

## Disadvatages
- Creation of the signed request requires code within the application

## Links
- [1585072449-conjur-home-authenticators.md](1585072449-conjur-home-authenticators.md)
- [1585068641-aws-fargate-iam-authn-conjur.md](1585068641-aws-fargate-iam-authn-conjur.md)
