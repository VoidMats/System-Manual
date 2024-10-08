
# Docker

To create a image locally 
```
$ sudo docker buildx build .                        # Create image from Dockerfile
$ sudo docker buildx build -f <filepath> .          # Create image from specified Dockerfile
$ sudo docker buildx build --build-arg VAR="test"   # Set variable which exist in Dockerfile
```

> Note 1. There are many build options. To get complete list [Read this](https://docs.docker.com/reference/cli/docker/buildx/build/)

> Note 2. Docker has changed from using 'docker build' to 'docker buildx build'. That does not effect the Dockerfile it self, just how the building is implemented and with extended support. Docker buildx was moved from experimental feature in Docker 20.10

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

## Install docker compose
```
 Debian system
$ sudo apt-get update
$ sudo apt-get install docker-compose-plugin
 RPM system
$ sudo yum update
$ sudo yum install docker-compose-plugin
```
Check the install with command - *$ docker compose version*

## Example docker-compose.yml
```
version: "3.3"
services:

  wfs1:
    image: docker_hub_account/application:master-latest
    container_name: application_name
    ports:
      - "5000:5000"
    restart: unless-stopped
    env_file:
      - .env
    environment:
      - ADDITIONAL_ENV=false
      - EXTRA_URL=https://some-url.com
```

# Traefik
Adding traefik to docker. Mount docker.sock, so traefik can listen to docker events. 
```
services: "3.3"

  traefik:
    image: traefik:v2.11
    container_name: traefik
    restart: always
    ports:
      - 80:80
      - 443:443
      # The Web UI (enabled by --api.insecure=true)
      - 8080:8080
    volumes:
      # So that Traefik can listen to the Docker events
      - /var/run/docker.sock:/var/run/docker.sock
      - ./traefik:/etc/traefik

  other_service:
    image: other_service
    container_name: other_service
    restart: always
    ports:
      - 3010:3010
    deploy:
      replicas: 2
    labels:
      traefik.enable: true
      traefik.http.routers.other_service.entrypoints: websecure
      traefik.http.routers.other_service.tls: true
      traefik.http.routers.other_service.tls.certresolver: production
      traefik.http.routers.other_service.rule: Host("other_service.domain.com")
      traefik.http.services.other_service.loadbalancer.server.port: 80
```