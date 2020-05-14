# 1589419925 cyberark-conjur-collection-ansible-run-through
#ansible #collection #conjur

This is running ansible-playbook on my mac, version below
```
$ ansible-playbook  --version
ansible-playbook 2.9.9
  config file = None
  configured module search path = ['/Users/acopeland/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/local/lib/python3.7/site-packages/ansible
  executable location = /usr/local/bin/ansible-playbook
  python version = 3.7.5 (default, Dec 19 2019, 00:05:55) [Clang 11.0.0 (clang-1100.0.33.16)]
```

First I create a file called `setup.sh` with the following contents:
```bash
export CONJUR_ACCOUNT=conjur
export CONJUR_APPLIANCE_URL=https://conjur-master
export CONJUR_CERT_FILE=/Users/acopeland/playground/cyberark.conjur/conjur-conjur.pem
export CONJUR_AUTHN_LOGIN=admin
export CONJUR_AUTHN_API_KEY=CYberark11@@

openssl s_client -showcerts -connect conjur-master:443 < /dev/null 2> /dev/null | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > conjur-$CONJUR_ACCOUNT.pem
```

Then I create file called `playbook.yml` with the following contents:
```yaml
---
- hosts: localhost
  tasks:
  - name: Lookup variable in Conjur
    debug:
      msg: "{{ lookup('cyberark.conjur.conjur_variable', '/path/to/secret') }}"
```

Then I  sourced the setup.sh

```bash
$ source setup.sh
```

And then ran the playbook
```
$ ansible-playbook playbook.yml
[WARNING]: No inventory was parsed, only implicit localhost is available
[WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match 'all'

PLAY [localhost] *************************************************************************************************************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************************************************************************************************
ok: [localhost]

TASK [Lookup variable in Conjur] *********************************************************************************************************************************************************************************************************
objc[29638]: +[__NSPlaceholderDate initialize] may have been in progress in another thread when fork() was called.
objc[29638]: +[__NSPlaceholderDate initialize] may have been in progress in another thread when fork() was called. We cannot safely call it or ignore it in the fork() child process. Crashing instead. Set a breakpoint on objc_initializeAfterForkError to debug.
ERROR! A worker was found in a dead state
```

Becuase mac has some issues with forking [found here](https://github.com/redhat-cop/openshift-applier/issues/132)

I then ran:
```bash
$ export OBJC_DISABLE_INITIALIZE_FORK_SAFETY=YES
$ ansible-playbook playbook.yml
[WARNING]: No inventory was parsed, only implicit localhost is available
[WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match 'all'

PLAY [localhost] *************************************************************************************************************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************************************************************************************************
ok: [localhost]

TASK [Lookup variable in Conjur] *********************************************************************************************************************************************************************************************************
fatal: [localhost]: FAILED! => {"msg": "An unhandled exception occurred while running the lookup plugin 'cyberark.conjur.conjur_variable'. Error was a <class 'urllib.error.URLError'>, original message: <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: self signed certificate in certificate chain (_ssl.c:1076)>"}

PLAY RECAP *******************************************************************************************************************************************************************************************************************************
localhost                  : ok=1    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0
```



## Links
