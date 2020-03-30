# 1585601586 conjur-troubleshooting-giving-ownership-from-synced-safe
#conjur #troubleshooting

You can grant someone owner of a safe that has been synced to conjur by loading the following policy:
```yaml
- !host safe-owner
- !permit
  role: !host safe-owner
  resource: !policy <vaultName>/<lobUser>/<safename>
  privileges: [ read, create, update, delete, execute ]
```

## Links
