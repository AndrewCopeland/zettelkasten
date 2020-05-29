# 1590712182 cyberark-psm-docker-ephermal-environments
#cyberark #psm #connection #component #docker #ephemeral #environments

```
If I need access to the admin user for the prod k8s clusters.
Then I retrieve the k8s admin user credential.
Then I log into the kubectl command line using this admin user credential.
This can be done on my local machine or a jump box.
```

The solution could be:
```
Then I must log into Cyberark, select the k8s admin account and click 'connect'
```

PSM with docker:
1. Psm would get the connect request with the k8s admin credentials.
2. Run a docker container with `kubectl`.
3. Get the PID of the docker container.
4. Login with the credentials.
5. Exec into the container
6. And give the user control


Advantages:
1. This allows us to give ephemeral environments to k8s admins.
2. The environments are recorded and auditable.
3. Only bash commands that are allowed are provided (All commmands could be whitelisted based off the base docker image)


If it could work with PSMP that would be pretty awesome also.

## Links
