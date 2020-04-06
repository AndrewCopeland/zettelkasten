# 1586189434 conjur-summon-ansible-implementation
#conjur #summon #integration

1. Generate the Host and API Key. Load by executing `conjur policy load root host_file_name.yml`
```yaml
---
- !host <host identity>
- !grant
  roles: 
    - !group <vault name>/<lob user>/<safe name #1>/delegation/consumers
    - !group <vault name>/<lob user>/<safe name #2>/delegation/consumers
  member: !host <host identity>
```

2. On the ansible host create `/etc/conjur.conf` with the following content:
```yaml
---
account: <conjur account>
plugins: []
appliance_url: https://<conjur appliance url>/
```

3. On the ansible host create `$HOME/.netrc`
```
machine https://conjur-master/authn
  login host/<host identity in step #1>
  password <host api key generated in step #1>
```

4. On the ansible host install summon:
```bash
curl -sSL https://raw.githubusercontent.com/cyberark/summon/master/install.sh | bash
````

5. On the anisble host install summon provider:
```bash
curl -sSL https://raw.githubusercontent.com/cyberark/summon-conjur/master/install.sh | bash
```

6. On the ansible host create a file called `secrets.yml` with the content:
```
<env var key>: !var <secret path>
<env var key>: !var <secret path>
<env var key>: !var <secret path>

e.g.
DB_ADDRESS: !var db/postgres/address
DB_USERNAME: !var db/postgres/username
DB_PASSWORD: !var db/postgres/password
```

7. Test summon by executing (this should print out the 3 values mentioned in the `secrets.yml`):
```bash
summon -f "<path to secrets.yml>/secrets.yml" env
```

8. execute the playbook with summon:
```bash
summon ansible-playbook test.yml
````

## Links
- [1584721461-conjur-appliance.md](1584721461-conjur-appliance.md)
