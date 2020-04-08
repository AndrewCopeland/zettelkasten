# 1586367984 conjur-kong-integration
#conjur #kong #integration

```bash
# start conjur container
IMAGE_NAME=captainfluffytoes/dap:11.2.1
docker container run -d \
     --name conjur-master \
     --network kong-net \
     --security-opt=seccomp:unconfined \
     -p 443:443 -p 5432:5432 -p 1999:1999 \
     $IMAGE_NAME

# configure conjur container
docker exec conjur-master \
     evoke configure master \
     --accept-eula --hostname conjur-master \
     --admin-password CYberark11@@ conjur

# bring up database
docker run -d --name kong-database \
               --network kong-net \
               -p 9042:9042 \
               cassandra:3

sleep 10

# bootstrap kong
docker run --rm \
     --network kong-net \
     -e "KONG_DATABASE=cassandra" \
     -e "KONG_CASSANDRA_CONTACT_POINTS=kong-database" \
     kong:latest kong migrations bootstrap

sleep 15

# actually setup the kong instance
docker run -d --name kong \
     --network kong-net \
     -e "KONG_DATABASE=cassandra" \
     -e "KONG_CASSANDRA_CONTACT_POINTS=kong-database" \
     -e "KONG_PROXY_ACCESS_LOG=/dev/stdout" \
     -e "KONG_ADMIN_ACCESS_LOG=/dev/stdout" \
     -e "KONG_PROXY_ERROR_LOG=/dev/stderr" \
     -e "KONG_ADMIN_ERROR_LOG=/dev/stderr" \
     -e "KONG_ADMIN_LISTEN=0.0.0.0:8001, 0.0.0.0:8444 ssl" \
     -p 8000:8000 \
     -p 8443:8443 \
     -p 127.0.0.1:8001:8001 \
     -p 127.0.0.1:8444:8444 \
     kong:latest

# bring up client
docker run -d --name client --network kong-net  --entrypoint "" ubuntu:latest sleep infinity


# from the client
docker exec -it client bash
apt update -y
apt install curl -y
apt install jq -y
# validate kong is running
curl -i http://kong:8001/


# setup the conjur service
curl -i -X POST \
     --url http://kong:8001/services/ \
     --data 'name=conjurApi' \
     --data 'url=https://conjur-master'

# setup the routes for the conjur service
curl -i -X POST \
     --url http://kong:8001/services/conjurApi/routes \
     --data 'hosts[]=conjur' \
     --data 'paths[]=/info' \
     --data 'strip_path=false' \
     --data 'methods[]=GET'

# try to make a request to the info endpoint
curl -i -X GET --url http://kong:8000/info --header 'Host: conjur'
```

## Links
