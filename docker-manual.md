
# Docker

To use a terminal within a container
```
$ sudo docker exec -it [kontainernamn] sh 
$ sudo docker-compose exec [service_name_in_docker_compose] sh
```
See actual status of all containers in a docker system. Update interval 
```
$ sudo docker stats
$ sudo docker container stats [CONTAINER...]
```
See how much disk space could be reclaimed from docker and how to reclaim it. It should be noted that prune will remove all containers which are in stop state.  
```
$ sudo docker system df
$ sudo docker system prune
``` 

# Docker-compose

# Traefik
