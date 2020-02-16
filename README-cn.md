> 如你如见，最近这个仓库在 GIthub 上变的流行。不久的将来，我们会进一步完善它。 
> 感谢你的支持。

点击 [网址](http://dockercheatsheet.painlessdocker.com).

*查看其他语言版本: [英语](README.md), [俄语](README.ru.md), [波斯语](README.fa.md)*

# 目录

   * [安装](#安装)
   * [Docker 仓库](#docker-仓库)
   * [运行容器](#运行容器)
   * [启动 &amp; 停止容器](#启动--停止容器)
   * [Getting Information about Containers](#getting-information-about-containers)
   * [网络](#网络)
   * [镜像安全性](#镜像安全性)
   * [清理 Docker](#清理-Docker)
   * [Docker Swarm](#docker-swarm)
   * [附录](#附录)

# 安装

## Linux

查看 [这里](https://docs.docker.com/install/#server)  获取更多信息

```
curl -sSL https://get.docker.com/ | sh
```

## Mac

查看 [这里](https://docs.docker.com/docker-for-mac/install/) 获取更多信息

使用下面连接下载 dmg 文件.

```
https://download.docker.com/mac/stable/Docker.dmg
```

##  Windows

查看 [这里](https://docs.docker.com/docker-for-windows/install/) 获取更多信息

通过 msi 文件安装:

```
https://download.docker.com/win/stable/InstallDocker.msi
```

# Docker 仓库

## 登录镜像仓库

```
docker login
```

```
docker login localhost:8080
```

## 从镜像仓库退出登录

```
docker logout
```

```
docker logout localhost:8080
```

## 搜索镜像

```
docker search nginx
```

```
docker search --filter stars=3 --no-trunc nginx
```

## 拉取镜像

```
docker image pull nginx
```

```
docker image pull eon01/nginx localhost:5000/myadmin/nginx
```

## 推送镜像

```
docker image push eon01/nginx
```

```
docker image push eon01/nginx localhost:5000/myadmin/nginx
```

# 运行容器

## 创建并运行一个简单的容器

> - 启动[ubuntu:latest](https://hub.docker.com/_/ubuntu/) 镜像
> - 绑定**容器**的 `80` 端口到**宿主机**的 `3000` 端口 
> - 将主机 `/data` 目录挂载到容器中
> - 注意: 在 **windows** 系统中，你需将 `-v ${PWD}:/data` 改为`-v "C:\Data":/data`

```
docker container run --name infinite -it -p 3000:80 -v ${PWD}:/data ubuntu:latest
```

## 创建容器

```
docker container create -t -i eon01/infinite --name infinite
```

## 运行容器

```
docker container run -it --name infinite -d eon01/infinite
```

## 重命名容器

```
docker container rename infinite infinity
```

## 删除容器

```
docker container rm infinite
```
容器只有停止后才可被删除，通过 ```docker stop``` 命令停止容器。为避免这个可在容器启动时加上```--rm``` 选项。     

## 更新容器配置

```
docker container update --cpu-shares 512 -m 300M infinite
```

## 在运行的容器中执行命令
```
docker exec -it infinite sh
```
上例中,如果报错可将 ```bash``` 可替换为 ```sh``` .

# 启动 & 停止容器

## 启动

```
docker container start nginx
```

## 停止
```
docker container stop nginx
```

## 重启
```
docker container restart nginx
```

## 暂停
```
docker container pause nginx

```

## 取消暂停

```
docker container unpause nginx
```

## 阻塞容器

```
docker container wait nginx
```

## 杀掉容器

```
docker container kill nginx
```

## 发送其他信号

```
docker container kill -s HUP nginx
```

## 连接现有容器

```
docker container attach nginx
```


# 获取容器相关详细

## 对于运行的容器

简写:        
```
docker ps
```
或者:      
```
docker container ls
```
## 查看所有容器   
```
docker ps -a
```
```
docker container ls -a
```

## 查看容器日志

```
docker logs infinite
```

## 追踪容器日志

```
docker container logs infinite -f
```

## 检查容器

```
docker container inspect infinite
```

```
docker container inspect --format '{{ .NetworkSettings.IPAddress }}' $(docker ps -q)
```

## 容器事件（？）

```
docker system events infinite
```

## 查看容器端口

```
docker container port infinite
```

## 运行进程

```
docker container top infinite
```

## Container Resource Usage

```
docker container stats infinite
```

## Inspecting changes to files or directories on a container’s filesystem

```
docker container diff infinite
```


## 操作镜像

## 列示镜像

```
docker image ls
```

## 构建镜像

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



## 删除镜像

```
docker image rm nginx
```

## Loading a Tarred Repository from a File or the Standard Input Stream

```
docker image load < ubuntu.tar.gz
```

```
docker image load --input ubuntu.tar
```

## 将镜像保存为 Tar 包

```
docker image save busybox > ubuntu.tar
```

## 展示镜像历史

```
docker image history  busybox
```

## 将容器保存为镜像

```
docker container commit nginx
```

## 给镜像打标签

```
docker image tag nginx eon01/nginx
```

## 推送镜像

```
docker image push eon01/nginx
```


# 网络

## 创建网络

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

## 删除某网络

```
docker network rm MyOverlayNetwork
```

## 列式现有网络

```
docker network ls
```

## 获取网络信息

```
docker network inspect MyOverlayNetwork
```

## 将运行中的容器接入某一网络

```
docker network connect MyOverlayNetwork nginx
```

## 启动容器时，将其接入某一网络

```
docker container run -it -d --network=MyOverlayNetwork nginx
```

## 将容器从某网络中断开连接

```
docker network disconnect MyOverlayNetwork nginx
```

## 暴露端口

使用 Dockerfile, 你可以暴露容器端口:

```
EXPOSE <port_number>
```

你还可以通过下面方式，将容器端口映射为宿主机端口:

docker run -p $HOST_PORT:$CONTAINER_PORT --name <container_name> -t <image>

例如

```
docker run -p $HOST_PORT:$CONTAINER_PORT --name infinite -t infinite
```

# 镜像安全性

## 构建安全镜像的一些指导建议

1. Prefer minimal base images
2. Dedicated user on the image as the least privileged user
3. Sign and verify images to mitigate MITM attacks
4. Find, fix and monitor for open source vulnerabilities
5. Don’t leak sensitive information to docker images
6. Use fixed tags for immutability
7. Use COPY instead of ADD
8. Use labels for metadata
9. Use multi-stage builds for small secure images
10. Use a linter

More detailed information on Snyk's [10 Docker Image Security Best Practices](https://snyk.io/blog/10-docker-image-security-best-practices/) blog

# 清理 Docker

## Removing a Running Container

```
docker container rm nginx
```

## Removing a Container and its Volume

```
docker container rm -v nginx
```

## Removing all Exited Containers

```
docker container rm $(docker container ls -a -f status=exited -q)
```


## Removing All Stopped Containers

```
docker container rm `docker container ls -a -q`
```

## Removing a Docker Image

```
docker image rm nginx
```

## Removing Dangling Images

```
docker image rm $(docker image ls -f dangling=true -q)
```

## Removing all Images

```
docker image rm $(docker image ls -a -q)
```

## Removing all untagged images

```
docker image rm -f $(docker image ls | grep "^<none>" | awk "{print $3}")
```

## Stopping & Removing all Containers

```
docker container stop $(docker container ls -a -q) && docker container rm $(docker container ls -a -q)
```

## Removing Dangling Volumes

```
docker volume rm $(docker volume ls -f dangling=true -q)
```

## Removing all unused (containers, images, networks and volumes)

```
docker system prune -f
```

## 清理所有

```
docker system prune -a
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


## 更新服务

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

# 附录

改文章首发于 [Painless Docker Course](http://painlessdocker.com)
