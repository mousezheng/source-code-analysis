狂神说 java docker [链接](https://www.bilibili.com/video/BV1og4y1q7M4?from=search&seid=8136680364557939141)

# docker

## 概述
主要学习 基础命令、原理、网络、数据卷、服务、集群、错误排查、日志
### 为什么出现
开发环境和生产环境、不同开发人员的开发环境都是不同的，由于环境不同造成项目部署运行繁琐易错，如何解决此问题呢？docker。

docker 提出隔离思想，通过容器对服务进行隔离。实现 devops 的利器

### 历史
- 2010 年 dotcloud 公司做 pass 的云计算服务
- 2013 年开源 docker
- 2014 年 4 月 docker1.0 发布

vm 抽象硬件，提供操作系统环境
docker 抽象核心库（m级别），提供容器（服务、应用）容器互相隔离，有自己的文件系统

### 其他
官网：https://www.docker.com/
文档：https://docs.docker.com/
docker hub：https://hub.docker.com/

docker 是一个 client-server 结构的系统，通过客户端访问守护进程的方式，守护进程执行命令

| 对比类型| docker|lxc linux 容器|vm|
|-|-|-|-|
|虚拟化类型|os虚拟化|模式虚拟化|硬件虚拟化|
|性能|约等于物理机|约等于物理机|5-20%损耗|
|隔离性|ns隔离，namespace| ns隔离|强|
|Qos| Cgroup 弱| Cgroup弱|强|
|安全性| 中|差|强|
|GuestOs|   linux|linux|全部|

## 命令

- docker version 版本信息
- docker info docker的系统信息，包括镜像容器的数量
- docker 命令 --help 命令说明 

### 镜像命令
- docker images 查看本地镜像
- docker search XX 搜搜镜像
- docker pull XX:1.0.1 拉取镜像
- docker rmi 镜像ID1 镜像ID2 删除镜像

### 容器命令
**docker run --参数 image 运行容器**

- --name='name' 容器名称
- -d 后台启动，如果没有前台进程则会自动停止
- -it 使用交互方式运行
- -p 80:8080 端口 
- -P 随机指定端口

```shell
docker run -it centos /bin/bash # 启动容器
exit #退出
```

**docker ps 查看运行容器**
- -a 列出全部
- -n=1 最近创建几个
- -q 只显示容器ID

**退出容器**
- exit 停止并退出容器
- ctrl + p + q 不停止退出

**docker rm 容器ID 删除容器**
docker rm 容器ID 可以删除停止的容器
docker rm -f $(docker ps -aq) 删除所有

**停止/启动**
- docker start 容器ID 
- docker restart 容器ID
- docker stop  容器ID
- docker kill 容器ID 强制停止   

**docker logs 查看容器日志**

docker exec -it 44f7c68d3885 /bin/bash -c "while true; do echo 'hello'; sleep 1; done;"

docker logs -ft --tail 10 容器ID
- -f 显示输出信息
- -t 显示时间戳
- --tail 显示最后 n 条

**docker top 容器ID 查看进程**

**docker inspect 容器ID 查看容器信息**

**进入容器**
- docker exec  进入容器，开启一个新的终端
- docker attach 进入正在执行的终端，不会启新的终端

**拷贝**
docker cp 容器ID:路径 主机 
容器关闭也可以拷贝。

**docker stats 容器ID 查看容器资源占用情况**

**docker commit**

docker commit 提交容器成为一个新的副本

docker commit -m="提交信息" -a="作者" 容器id 目标镜像名:[TAG]

### 操作命令
## 可视化工具

portainer 

rancher

## 容器数据卷

通过 -v 命令将本地文件目录映射到容器文件目录中 
docker run -v /home/test:/home centos 

docker inspect  mounts 中会记录挂在信息

### 具名挂在/匿名挂在
- -v 容器内路径             匿名挂在，会在宿主机上随机生成一个目录
- -v 宿主机目录:容器内路径    具名挂载 
- -v /宿主机目录:容器内路径   指定路径挂载

- -v XX:容器内路径:ro   目录只读，由外部修改
- -v XX:容器内路径:rw   目录只写

- docker volume 管理卷
- run --volume-from 可以复用已有容器的文件映射，文件互通

## DockerFile

用来构建镜像
构建步骤
1. 编写dockerfile
2. docker build -f XX -t XX/XX:1.0
3. docker push 发布到 docker hub （需要 docker login，可以发布到阿里云服务）

### docker file 编写
- 关键字必须大写
- 顺序执行 
- 每个命令会提交一层


1. FROM：基础镜像
2. MAINTAINER: 维护者信息
3. RUN：构建镜像时执行的命令
4. ADD：将本地文件添加到容器中，tar类型文件会自动解压(网络压缩资源不会被解压)，可以访问网络资源，类似wget
5. COPY：功能类似ADD，但是是不会自动解压文件，也不能访问网络资源
6. CMD：构建容器后调用，也就是在容器启动时才进行调用。
7. ENTRYPOINT：配置容器，使其可执行化。配合CMD可省去"application"，只使用参数。
8. LABEL：用于为镜像添加元数据
9. ENV：设置环境变量
10. EXPOSE：指定于外界交互的端口
11. VOLUME：用于指定持久化目录
12. WORKDIR：工作目录，类似于cd命令
13. USER:指定运行容器时的用户名或 UID，后续的 RUN 也会使用指定用户。使用USER指定用户时，可以使用用户名、UID或GID，或是两者的组合。当服务不需要管理员权限时，可以通过该命令指定运行用户。并且可以在之前创建所需要的用户
14. ARG：用于指定传递给构建运行时的变量
15. ONBUILD：用于设置镜像触发器
[参考文档](https://www.cnblogs.com/panwenbin-logs/p/8007348.html)

## docker 网络原理

每个容器都有自己的ip，通过桥接模式，docker0 默认路由器，veth-pair
--link 容器名 容器可以通过 ping 容器名（通过 /etc/hosts）

### 自定义网络
docker network

模式
- bridge 网桥docker默认
- none 不配置网络
- host 和宿主共享网络
- contain 容器内网络连通（用的少，有局限）

docker network create 
--driver bridge         #模式
--subnet 192.168.0.0/16     # 子网
--gateway 192.168.0.1  # 网关
mynet   #网络名

## docker composer

通过配置运行多个容器

使用步骤
1. DockerFile 镜像构建方式
2. docker-composer.yml 容器启动配置
3. docker-composer up 启动

### docker-composer.yml
核心三层
1. version  版本
2. service 服务
   1. 名称
   2. images
   3. build
   4. network
3. 其他（volums，network，configs）


## 编排 集群

节点 管理节点、工作节点
raft 协议  保证大多数节点存活才可用
弹性、扩缩容、集群
docker service
### docker swarm
### k8s


## CICD Jenkins
