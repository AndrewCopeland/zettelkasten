# 1584722235 conjur-policy
#conjur #policy

Everything within conjur can be defined as a policy. Below is an example of the policy.
```yaml
- !host app1
- !policy
  id: app1
  owner: !host app1
  body:
  - &secrets
    - !variable secret1
    - !variable secret2
  - !group secret-consumers
  - !permit
    role: !group secret-consumers
    resources: *secrets
    privilege: [ read, execute ]
```

Anything that starts with a `!` is considered a policy statement reference. 

## Links
- [1584721461-conjur-appliance.md](1584721461-conjur-appliance.md)
- [1584722460-conjur-resource-variable.md](1584722460-conjur-resource-variable.md)
