# Docker documentation

***



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