# 1584721724 conjur-authn-oidc
#exampletag

The conjur `authn-oidc` authenticator allows the ability to authenticate using a JWT token.
- [example of using okta with conjur](1585089211-conjur-okta-authentication-bash.md)

## Advantages
- No secret zero
- Identies can be either user or application specific
- Integrates with okta relitively easly

## Disadvatages
- application code is required to interface with the OIDC provider (however most of the time this is documented well via the vendor)

## Links
- [1585072449-conjur-home-authenticators.md](1585072449-conjur-home-authenticators.md)
- [1585089211-conjur-okta-authentication-bash.md](1585089211-conjur-okta-authentication-bash.md)
