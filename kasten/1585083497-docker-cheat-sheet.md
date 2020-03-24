# 1585083497 docker-cheat-sheet
#docker #cheat-sheet

List all running docker contains
```bash
docker ps
```

Get logs of docker container
```bash
docker logs -f <container name> 
# to follow from bottom
docker logs -f --since 1m <container name>
```

Exec into a docker container
```bash
docker exec -it <container name> bash
# or if bash is not on the container
docker exec -it <container name> sh
```

## Links
