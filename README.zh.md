> 如你如见，最近这个仓库在 GIthub 上变的流行。不久的将来，我们会进一步完善它。   
> 感谢你的支持。

Work adapted from [Painless Docker](https://faun.dev/sensei/academy/painless-docker-2nd-edition-a19d29-c19c66-29f3a8-0/) (2nd edition).


*查看其他语言版本: [英语](README.md), [俄语](README.ru.md), [波斯语](README.fa.md)*

# 目录

   * [安装](#安装)
   * [Docker 仓管中心和仓库](#Docker-仓管中心和仓库)
   * [运行容器](#运行容器)
   * [启动 &amp; 停止容器](#启动--停止容器)
   * [获取容器相关详细](#获取容器相关详细)
   * [网络](#网络)
   * [镜像安全性](#镜像安全性)
   * [清理 Docker](#清理-Docker)
   * [Docker Swarm](#docker-swarm)
   * [Docker Compose](#docker-compose)
   * [实用命令](#实用命令)
   * [附录](#附录)

# 安装

## Linux

查看 [这里](https://docs.docker.com/install/#server)  获取更多信息。

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

# Docker 仓管中心和仓库

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

## 查看运行的容器

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

## 查看容器事件

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

## 查看容器资源使用情况

```
docker container stats infinite
```

## 检查容器文件系统上文件或目录的更改情况

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

## 从压缩文件中导入镜像

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

## 列示现有网络

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

1. 选择最精简的基础镜像
2. 创建用户并分配使其用镜像基础权限
3. 署名并校验镜像以避免中间人攻击（MITM） 攻击
4. 寻找、修复和关注开源漏洞
5. 不要在镜像中泄露敏感信息
6. 使用明确固定的镜像标签
7. 使用 COPY 代替 ADD
8. 为元数据创建标签
9. 采用多阶段构建以获得小且安全的镜像
10. 使用 linter

详细内容参考 Snyk 的 [10 Docker Image Security Best Practices](https://snyk.io/blog/10-docker-image-security-best-practices/) blog

# 清理 Docker

## 删除容器

```
docker container rm nginx
```

## 删除容器和其数据卷

```
docker container rm -v nginx
```

## 删除所有退出（Exited）容器

```
docker container rm $(docker container ls -a -f status=exited -q)
```


## 删除所有退出（Stopped）容器

```
docker container rm `docker container ls -a -q`
```

## 删除镜像

```
docker image rm nginx
```

## 删除悬空（Dangling）镜像

```
docker image rm $(docker image ls -f dangling=true -q)
```

## 删除所有镜像

```
docker image rm $(docker image ls -a -q)
```

## 删除所有无标签镜像

```
docker image rm -f $(docker image ls | grep "^<none>" | awk "{print $3}")
```

## 停止 & 删除所有镜像

```
docker container stop $(docker container ls -a -q) && docker container rm $(docker container ls -a -q)
```

## 删除悬空（Dangling）数据卷

```
docker volume rm $(docker volume ls -f dangling=true -q)
```

## 删除所有 （容器、镜像、网络和数据卷）

```
docker system prune -f
```

## 清理所有

```
docker system prune -a
```

# Docker Swarm

## 安装 Docker Swarm

```
curl -ssl https://get.docker.com | bash
```


## 初始化 Swarm

```
docker swarm init --advertise-addr 192.168.10.1
```


## 将工作节点加入 Swarm

```
docker swarm join-token worker
```


## 将管理节点加入 Swarm

```
docker swarm join-token manager
```



## 列示服务

```
docker service ls
```


## 列示节点

```
docker node ls
```


## 新建服务

```
docker service create --name vote -p 8080:80 instavote/vote
```


## 列示 Swarm 任务

```
docker service ps
```


## 伸缩服务

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

# Docker Compose

```
docker compose up -d              # 后台启动服务
docker compose down               # 停止并删除容器
docker compose down -v            # 停止并删除容器和卷
docker compose logs -f            # 查看日志
docker compose ps                 # 列出运行中的服务
docker compose exec app sh        # 在服务中执行命令
docker compose build --no-cache   # 无缓存重建
```

# 实用命令

## 在容器和主机之间复制文件

```
docker cp container:/path/file /host/path
docker cp /host/path container:/path/file
```

## 查看资源使用情况

```
docker stats
docker system df
```

## 获取容器IP地址

```
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container_name
```

## 以指定用户身份运行

```
docker exec -u root -it container_name sh
```

## 快速清理

```
docker system prune -a --volumes  # 删除所有未使用的数据
```

# 附录

该文章首发于 [Painless Docker Course](http://painlessdocker.com)
