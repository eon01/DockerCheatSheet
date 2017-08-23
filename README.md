# Installation

## Linux

```
curl -sSL https://get.docker.com/ | sh
```

## Mac

Use this link to download the dmg.

```
https://download.docker.com/mac/stable/Docker.dmg
```

##  Windows

Use the msi installer:

```
https://download.docker.com/win/stable/InstallDocker.msi
```

# Docker Registries & Repositories

## Login to a Registry

```
docker login
```

```
docker login localhost:8080
```

## Logout from a Registry.

```
docker logout 
```

```
docker logout localhost:8080
```

## Searching an Image

```
docker search nginx
```

```
docker search nginx --stars=3 --no-trunc busybox
```

## Pulling an Image

```
docker pull nginx
```

```
docker pull eon01/nginx localhost:5000/myadmin/nginx
```

## Pushing an Image

```
docker push eon01/nginx
```

```
docker push eon01/nginx localhost:5000/myadmin/nginx
```

# Running Containers

## Creating a Container

```
docker create -t -i eon01/infinite --name infinite
```

## Running a Container 

```
docker run -it --name infinite -d eon01/infinite
```

## Renaming a Container

```
docker rename infinite infinity
```

## Removing a Container

```
docker rm infinite
```

## Updating a Container

```
docker update --cpu-shares 512 -m 300M infinite
```

# Starting & Stopping Containers

## Starting

```
docker start nginx
```

## Stopping
```
docker stop nginx
```

## Restrating
```
docker restart nginx
```

## Pausing
```
docker pause nginx

```

## Unpausing

```
docker unpause nginx
```

## Blocking a Container

```
docker wait nginx
```

## Sending a SIGKILL

```
docker kill nginx
```

## Connecting to an Existing Container

```
docker attach nginx
```


# Getting Information about Containers

## Running Containers

```
docker ps
```

```
docker ps -a
```

## Container Logs

```
docker logs infinite
```

## Inspecting Containers 

```
docker inspect infinite
```

```
docker inspect --format '{{ .NetworkSettings.IPAddress }}' $(docker ps -q)
```

## Containers Events

```
docker events infinite
```

## Public Ports

```
docker port infinite
```

## Running Processes

```
docker top infinite
```

## Container Resource Usage

```
docker stats infinite
```

## Inspectting changes to files or directories on a container’s filesystem

```
docker diff infinite
```


## Manipulating Images

## Listing Images

```
docker images
```

## Building Images

``` 
docker build . 
```

``` 
docker build github.com/creack/docker-firefox 
```

``` 
docker build - < Dockerfile 
```

``` 
docker build - < context.tar.gz 
```

``` 
docker build -t eon/infinite . 
```

``` 
docker build -f myOtherDockerfile . 
```

``` 
curl example.com/remote/Dockerfile | docker build -f - . 
```



## Removing an Image

``` 
docker rmi nginx
``` 

## Loading a Tarred Repository from a File or the Standard Input Stream

```
docker load < ubuntu.tar.gz
```

```
docker load --input ubuntu.tar
```

## Save an Image to a Tar Archive

```
docker save busybox > ubuntu.tar
```

## Showing the History of an Image

```
docker history 
```

## Creating an Image From a Container

``` 
docker commit nginx
``` 

## Tagging an Image

```
docker tag nginx eon01/nginx
```

## Pushing an Image

```
docker push eon01/nginx
```


# Networking

## Creating Networks

```
docker network create -d overlay MyOverlayNetwork
```

```
docker network create -d bridge MyBridgeNetwork
```

```
docker network create -d overlay \
  --subnet=192.168.0.0/16 \
  --subnet=192.170.0.0/16 \
  --gateway=192.168.0.100 \
  --gateway=192.170.0.100 \
  --ip-range=192.168.1.0/24 \
  --aux-address="my-router=192.168.1.5" --aux-address="my-switch=192.168.1.6" \
  --aux-address="my-printer=192.170.1.5" --aux-address="my-nas=192.170.1.6" \
  MyOverlayNetwork
```

## Removing a Network

```
docker network rm MyOverlayNetwork
```

## Listing Networks

```
docker network ls
```

## Getting Information About a Network

```
docker network inspect MyOverlayNetwork
```

## Connecting a Running Container to a Network

```
docker network connect MyOverlayNetwork nginx
``` 

## Connecting a Container to a Network When it Starts

``` 
docker run -it -d --network=MyOverlayNetwork nginx
``` 

## Disconnecting a Container from a Network

```
docker network disconnect MyOverlayNetwork nginx
```


# Cleaning Docker

## Removeing a Running Container

```
docker rm nginx
```

## Removing a Container and its Volume

```
docker rm -v nginx
```

## Removing all Exited Containers

```
docker rm $(docker ps -a -f status=exited -q)
```


## Removing All Stopped Containers

```
docker rm `docker ps -a -q`
```

## Removing a Docker Image

```
docker rmi nginx
```

## Removing Dangling Images

```
docker rmi $(docker images -f dangling=true -q)
```

## Removing all Images

```
docker rmi $(docker images -a -q)
```

## Stopping & Removing all Containers

```
docker stop $(docker ps -a -q) && docker rm $(docker ps -a -q) 
```

## Removing Dangling Volumes

```
docker volume rm $(docker volume ls -f dangling=true -q)
```

# Docker Swarm 

## Installing Docker Swarm

```
curl -ssl https://get.docker.com | bash
```


## Initializing the Swarm

```
docker swarm init --advertise-addr 192.168.10.1
```


## Getting a Worker to Join the Swarm

```
docker swarm join-token worker
```


## Getting a Manager to Join the Swarm

```
docker swarm join-token manager
```



## Listing Services

```
docker service ls
```


## Listing nodes

```
docker node ls
```


## Creating a Service

```
docker service create --name vote -p 8080:80 instavote/vote
```


## Listing Swarm Tasks

```
docker service ps
```


## Scaling a Service


```
docker service scale vote=3
```


## Updating a Service 

```
docker service update --image instavote/vote:movies vote
```

```
docker service update --force --update-parallelism 1 --update-delay 30s nginx
```

```
docker service update --update-parallelism 5--update-delay 2s --image instavote/vote:indent vote
```

```
docker service update --limit-cpu 2 nginx
```

```
docker service update --replicas=5 nginx
```
