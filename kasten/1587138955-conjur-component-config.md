# 1587138955 conjur-component-config
#conjur #component

It would be nice to have a standardized why of creating conjur components.
This would allow us to define specific endpoints for a component that correspond to privileges a specific user has access to.

So given this policy:
```yaml
# host that will run eaas
- !host eaas-component

# policy that will repersent the eaas and webservice
- !policy
  id: eaas
  owner: !host eaas-component
  body:
  - !webservice
  - !group encrypt
  - !group decrypt
  - !group all

  - !permit
    role: !group encrypt
    resource: !webservice
    privileges: [ read, encrypt ]

  - !permit
    role: !group decrypt
    resource: !webservice
    privileges: [ read, decrypt ]

  - !grant
    roles:
    - !group encrypt
    - !group decrypt
    member: !group all

# host that will use eaas
- !host use-eaas
- !grant
  role: !group eaas/all
  member: !host use-eaas
```

And providing this configuration to the conjur component:
```yaml
version: "1"
services:
  eaas:
    conjur_config:
      appliance_url: https://conjur-master
      account: conjur
      authn_login: host/eaas-component
      authn_api_key: fhhfhls88shjslsl83hlaslgfvbkls83bas83
      ignore_untrusted_cert: yes
    component_config:
      conjur_webservice: eaas
      endpoint: eaas
      execute: python3 eaas.py --action <action> --data <data> --key <key>
      endpoints:
        # these relates to `privileges` on the webservice.
        encrypt: [ POST ]
        decrypt: [ POST ]
```



`!host use-eaas` should be able to perform the following:
```bash
curl -H "${CONJUR_HEADER}" --data "DATA I NEED ENCRYPTED" <conjur component>/v1/eaas/encrypt
# should return the encrypted messs
```

## Links
- [1589824045-conjur-component-assume-iam-role.md](1589824045-conjur-component-assume-iam-role.md)
