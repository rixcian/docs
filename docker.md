# Docker documentation

***

### .dockerignore file

It helps to avoid unnecessarily sending large or sensitive files and directories to the daemon and potentially adding them to images.



## Useful commands

### Docker images

**Build image from Dockerfile**

`$ docker build . -t <image_name>`

`$ docker build . -t rixcian/<image_name>`

**Remove image**

`$ docker rmi <image_name>`

**Remove all images**

`$ docker rmi $(docker images -q)`

` $ docker rmi $(docker images -q) --force`



### Docker containers

**Run docker container from image**

`$ docker run --name <container_name> -p <local_port>:<internal_port> -d rixcian/<image_name>`

**Stop container**

`$ docker stop <container_name>`

**Stop all containers**

`$ docker stop $(docker ps -a -q)`

**Run already created docker container**

`$ docker start <container_name> `

**Show all running containers**

`$ docker ps`

**Show all containers (even those who are stopped)**

` $ docker ps -a` 

**Remove container**

`$ docker rm <container_name>`

**Remove all containers**

`$ docker rm $(docker ps -a -q)`

**Execute bash in container**

`$ docker exec -it <container_name> /bin/bash`



### Docker volumes

**List all volumes**

`$ docker volume ls`

**Create a volume**

`$ docker volume create <volume_name>`

**Inspect volume**

`$ docker volume inspect <volume_name>`

**Delete volume**

`$ docker volume rm <volume_name> `

**Delete all unused volumes**

`$ docker volume prune`





## Useful tools

- **docker-slim** - minifying docker image containers
  - https://github.com/docker-slim/docker-slim