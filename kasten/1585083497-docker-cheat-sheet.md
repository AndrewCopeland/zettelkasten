# 1585083497 docker-cheat-sheet
#docker #cheat-sheet

list all running docker contains
```bash
docker ps
```

get logs of docker container
```bash
docker logs -f <container name> 
# to follow from bottom
docker logs -f --since 1m <container name>
```

exec into a docker container
```bash
docker exec -it <container name> bash
# or if bash is not on the container
docker exec -it <container name> sh
```

force remove a docker container
```bash
docker rm -f <container name>
```

an ephermeral ubuntu container
```bash
 docker run -it --entrypoint bash ubuntu:18.04
```



## Links