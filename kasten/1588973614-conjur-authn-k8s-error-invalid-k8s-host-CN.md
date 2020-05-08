# 1588973614 conjur-authn-k8s-error-invalid-k8s-host-CN
#conjur #authn-k8s #error #invalid-k8s-host


When authenticating using an application I am recieveing the following error message:
```
Authentication Error: #<ArgumentError: Invalid K8s host CN: host.nexus-member. Must end with namespace.controller.id>
/opt/conjur/possum/app/domain/authentication/authn_k8s/common_name.rb:46:in `validate!'
```

## Solution
1. Looks like this error can be returned if using `authn-k8s` annotation authentication, to resolve this issue you can upgrade Conjur or use the old way of defining `authn-k8s` hosts. e.g. `conjur/authn-k8s/<serviceId>/apps/<namespace>/<resource type>/<resource name>`

## Links
