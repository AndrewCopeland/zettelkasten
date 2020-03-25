# 1585155574 conjur-authn-azure-implementation
#conjur #authn-azure

The following policy will need to be loaded to configure conjur to use the azure authenticator.
```yaml
- !policy
  id: conjur/authn-azure/<service id>
  body
  - !webservice
  - !variable provider-uri
  - !group apps
  - !permit
    role: !group apps
    resource: !webservice
    privileges: [ read, authenticate ]
```

Then populate the provider-uri variable
```bash
conjur variable values add conjur/authn-azure/provider-uri "https://sts.windows.com/<tenant id>/"
```

Then enable to the authenticator by setting the CONJUR_AUTHENTICATORS environment variable.
Exec into the conjur appliance and perform the following command:
```bash
evoke variable set CONJUR_AUTHENTICATORS "authn,authn-azure/<service id>"
```

At this point the authenticator should be configured and enabled. You can check this by performing the following command:
```bash
curl -k https://<conjur appliance url>/info
```

Now we want to create a host that will have the ability to authenticate to the `authn-azure` authenticator we have enabled.
```yaml
# host with the appropriate annotations
- !host
  id: azure-managed-identity
  annotations:
    authn-azure/subscription-id: <sub id of the managed identity>
    authn-azure/resource-group: <resource group of the managed identity>
    authn-azure/user-managed-identity: <name of the managed identity>

# actually give the host ability to use the authn-azure authenticator
- !grant
  role: !group conjur/authn-azure/<service id>/apps
  member: !host azure-managed-identity
```

Now that the authenticator is configured and enabled and the host is configured correctly. We now want to log into an Azure VM that has the user managed identity associated with it and execute the following.
```bash
# retrieve access token from the azure metadata service
access_token=$(curl -s -H "Metadata: true" "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&client_id=<managed identity client ID>&resource=https://management.azure.com/" | jq -r ".access_token")

# authenticate to conjur using the access token
conjur_session_token=$(curl -k -H "Content-Type: application/x-www-form-urlencoded" --data "jwt=$access_token" "https://<conjur appliance url>/authn-azure/<service id>/<conjur account>/host%2Fazure-managed-identity/authenticate")
```

## Links
- [1585072449-conjur-home-authenticators.md](1585072449-conjur-home-authenticators.md)
