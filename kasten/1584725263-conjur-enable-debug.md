# 1584725263 conjur-enable-debug
#conjur #debug

You can enable debuging in conjur by executing `evoke variable set CONJUR_LOG_LEVEL debug` on the conjur container.
Also you can enable debug by concatenating `CONJUR_LOG_LEVEL=debug` to the `/opt/conjur/etc/conjur.conf` file and then restarting the conjur service using `sv restart conjur`

## Links
- [1584721461-conjur-appliance.md](1584721461-conjur-appliance.md)
