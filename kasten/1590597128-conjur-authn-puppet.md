# 1590597128 conjur-authn-puppet
#conjur #puppet #authenticator

Creating an authenticator for puppet agents. The puppet agent could generate a JWT using its private key (that gets assigned per agent).

1. Figure out how to obtain the private key from the puppet agent.
2. Figure out how to obtain the certificate we can use to validate the signed request.
3. What meta-data does the puppet agent have access too.
4. What meta-data does the puppet master have access too.
5. Will conjur need to connect to the puppet API or could we just validate the JWT and use a 1 to 1 mapping of payload attribues and host annotations.
6. How will we know the conjur host ID 
 
## Links
