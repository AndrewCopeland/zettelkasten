# 1587142600 cyberark-conjur-vault-sync-install
#cyberark #conjur #sync #install #syncronizer

This document will go through setting up the conjur vault syncronizer.

## Creating the Sync user in Cyberark
We will need to create the syncronizer vault user. To do this open the PrivateArk client go to `Tools > Administrative Tools > Users and Groups > New > User` and create a user called `Sync_<Sync machine hostname>` 

| Info | Vault |
| ---- | ----- |
| Username | Sync_<Sync machine hostname> |
| User type | APPProvider |
| Password | Any password |
| Must change password at next login | Uncheck|
| Password never expires | Check |

Add the user created above as a member to the PVWAConfig safe with the following permissions:
* List Files
* Retrieve Files
* Access Safe without Confirmation

## Validating the needed platform exists
Open the PVWA (Cyberark Website) and navigate to `ADMINISTATION > Platform Management`
Once here activate both of the platforms `CyberArk Vault` and `Conjur Host`.

## Creating the ConjurSync safe
Open the PVWA and navigate the `Policies > Access Control (Safes) > Add Safe`. Create the safe with the name of `ConjurSync`.

After creating this safe add the Sync user we created above as a member to this safe with the following permissions:
* Use Account
* Retrieve Account
* List Account
* Add Account
* Update account content
* Update account properties
* Access Safe without confirmation
* Create folder
* Delete Folder

## Installing the Vault Conjur Syncronizer
After downloading and unzipping the Vault Conjur Syncronizer from the support vault perform the following command in an Administative powershell prompt:
`.\V5SynchronizerInstallation.ps1`

Fill in the information that is prompted correctly and eventually the service should be running.

You can find the installation logs in:
`<Synchronizer directory>/Logs/Installation.log`

## Post Installation





## Links
- [1588949676-cyberark-conjur-sync-unable-to-detect-conjur-server-version.md](1588949676-cyberark-conjur-sync-unable-to-detect-conjur-server-version.md)
