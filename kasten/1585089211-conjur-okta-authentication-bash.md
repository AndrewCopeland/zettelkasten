# 1585089211 conjur-okta-authentication-bash
#conjur #authn-oidc #bash

Authenticate to okta and then authenticate to conjur to retrieve secret.

```bash
#!/bin/bash
# --- AUTHENTICATE TO CONJUR USING OKTA ---
# Prompt for OKTA password and then generate OKTA push notification to username via OIDC.
# Acquire id_token from OKTA and use it to generate a valid Conjur token.

# MANDATORY ARGUMENTS:
# conjur_uri: The conjur appliance url
# account_id: The conjur organizational account, can be found using the command 'curl $conjur_uri/info'
# username: username of the entity authenticating to conjur

# BASH DEPENDANCIES REQUIRED:
# curl, jq, printf, egrep, cut, base64, trim

okta_service_id="cyberark-okta"
conjur_uri=$1
account_id=$2
username=$3
default_orgUrl="https://cyberark.okta.com"

echo -n "Password: "
read -s password

# --- PRIMARY OKTA AUTHENTICATION ---
# this should return status, statusToken and pushFactorId
# echo "Doing primary authentication..."
orgUrl="${orgUrl:-${default_orgUrl}}"
raw=`curl -s -H "Content-Type: application/json" -d "{\"username\": \"${username}\", \"password\": \"${password}\"}" ${orgUrl}/api/v1/authn`
status=`echo ${raw} | jq -r '.status'`
stateToken=`echo $raw | jq -r '.stateToken'`
pushFactorId=`echo $raw | jq -r '.["_embedded"].factors[0].id'`
if [ $stateToken == "null" ]; then
  echo -e "\nAuthentication failed."
  exit 3
fi
echo "Congratulations! You got a stateToken: ${stateToken} That's used in a multi-step authentication flow, like MFA."

# --- VERIFY PUSH FACTOR ID ---
# using the pushFactorId & stateToken from primary authentication
# send a push factor notification and return a sesstion token
echo -e "\nSending Okta Verify push notification..."
status="MFA_CHALLENGE"
tries=0
while [[ ${status} == "MFA_CHALLENGE" && ${tries} -lt 10 ]]
do
      verifyAndPoll=`curl -s -H "Content-Type: application/json" -d "{\"stateToken\": \"${stateToken}\"}" ${orgUrl}/api/v1/authn/factors/${pushFactorId}/verify`
      status=`echo ${verifyAndPoll} | jq -r '.status'`
      tries=$((tries+1))
      echo "Polling for push approve..."
      sleep 6
done
sessionToken=`echo ${verifyAndPoll} | jq -r '.sessionToken'`
echo "Fetching ID Token using sessionToken ${sessionToken}"


# --- AUTHORIZE AND RETRIEVE JWT ID TOKEN ---
# Using the sessionToken returned after verifying the push factor id
# authorize to OKTA and obtain the id_token from the header of the response
client_id="<OKTA client ID goes here>"
redirect_uri="https://invalidurl.corp.example.com"
url=$(printf "%s/oauth2/v1/authorize?sessionToken=%s&client_id=%s&scope=openid+email&response_type=id_token&response_mode=fragment&nonce=%s&redirect_uri=%s&state=%s" \
      $orgUrl \
      $sessionToken \
      $client_id \
      "staticNonce" \
      $redirect_uri \
      "staticState")
raw=`curl -s -v  ${url} 2>&1`
id_token=$(echo "${raw}" | egrep -o '.*ocation: .*id_token=[[:alnum:]_\.\-]*' | cut -d \= -f 2)


# --- AUTHENTICATE TO CONJUR ---
# Use id_token to acquire a valid Conjur token to perform REST API calls to Conjur
access_token=$(curl -s -k -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "id_token=${id_token}" ${conjur_uri}/authn-oidc/${okta_service_id}/${account_id}/authenticate)
access_token_encoded=$(echo -n "${access_token}" | base64 | tr -d '\r\n')

variable_id="path/to/your/secret"
secret_value=$(curl -k -fs -H "Authorization: Token token=\"$access_token_encoded\"" ${conjur_uri}/secrets/${account_id}/variable/${variable_id})
echo "$secret_value"
```



## Links
- [1584721724-conjur-authn-oidc.md](1584721724-conjur-authn-oidc.md)
