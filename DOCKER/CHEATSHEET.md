# Docker Cheat Sheet

## Basic Commands
docker --version              # check version
docker pull <image>          # download image
docker images                # list local images
docker ps                    # list running containers
docker ps -a                 # list all containers

## Running Containers
docker run <image>                        # run once
docker run -d <image>                     # run detached
docker run -it <image> bash               # interactive
docker run --name <name> <image>          # with name
docker run -p 8080:80 <image>             # port mapping
docker run -e VAR=value <image>           # environment variables

## Container Management
docker stop <container>      # stop gracefully
docker kill <container>      # stop forcefully
docker restart <container>   # restart
docker rm <container>        # remove container
docker logs <container>      # see logs
docker logs -f <container>   # follow logs
docker exec -it <container> bash  # run command in container

## Building Images
docker build -t <name:tag> .      # build image
docker build -f Dockerfile.prod . # specific dockerfile

## Image Management
docker images                # list images
docker rmi <image>          # remove image
docker inspect <image>      # image details
docker history <image>      # image layers

## Cleanup
docker rm $(docker ps -aq)                    # remove all stopped
docker rmi $(docker images -q)                # remove all images
docker system prune                          # cleanup everything