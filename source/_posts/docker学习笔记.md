---
title: docker学习笔记
date: 2016-11-02 10:07:48
tags: docker
categories: docker
---


###简介
>容器与管理程序虚拟化有所不同，管理程序虚拟化通过中间层将一台或多台独立的机器虚拟运行于物理硬件之上，而容器则是直接运行在操作系统内核之上的用户空间。
>由于客居于操作系统，容器智能运行与底层宿主机相同或相似的操作系统。

###docker组件
- Docker客户端和服务端

    docker是C/S架构的程序
- Docker镜像

    镜像相当于容器的"源代码"。体积小，易于分享，存储和跟新。
- Registry
    
    docker用registry来保存用户构建的镜像。类似gitHub,docker仓库是docker hub.
- Docker容器

    包含了：
    - 一个镜像格式
    - 一系列标注操作
    - 一个执行环境

###docker能做什么
- 加速本地开发和构建流程，使其更加高效，更加轻量化。
- 能够让独立服务或应用程序在不同的环境中，得到相同的运行结果。
- 用docker创建隔离的环境来进行测试。
- 构建一个多用户的平台Pass
- 高性能、超大规模的宿主机部署
- ......

### docker相关命令
docker run -it  --name  hahah  ubuntu /bin/bash


查找本地是否存在ubuntu:latest镜像
存在就启动

不存在就从docker hub查找镜像下载保存到本地

使用ubuntu镜像创建一个容器


--name  指定容器别名

-d 将容器放到后台运行


docker rm 删除容器

删除所有容器 

<pre>docker rm `docker ps -a -q`</pre>

docker start 启动容器

docker restart 重启容器

docker stop $name  停止容器

docker attach  进入容器会话

docker logs 获取守护容器的日志

docker logs -f  跟踪守护式容器的日志


- 查看当前系统中的容器列表

 `docker ps -a` 


- 查看容器内的进程
    
    `docker top $name`
    
- 在容器内部运行进程
    
    - 在容器中运行后台任务
    
     docker exec -d $name touch /etc/new_config_file
     
     -d 表叔运行一个后台进程
     
    - 在容器内运行交互命令
    
        `docker exec -i -t $name /bin/bash`
        
        -i -t 创建tty并捕捉STDIN
    
- 自动重启容器

    `docker run --restart=always --name $name -d  ubuntu /bin/sh  -c "while true; do echo hello world; sleep 1; done"`
        
    --restart=on-failure
    
    --restart=on-failure:3

- 深入容器，对容器进行详细的检查返回其配置信息
    
    docker inspect $name
    
###构建镜像
####查找镜像
`docker search ubuntu`

####构建镜像

- docker commit 命令
- docker build命令和Dockerfile文件

#####docke commit
1. 进入一个基础容器
2. 修改容器内容
3. docker commit 
    `docker commit -m '提交信息'  --authon="justudy" $name     justudy/nginx:webserver`
    
    justudy/nginx指定了镜像的用户名和仓库名，并增加了一个webserver标签

    可以使用 docker inspect justudy/nginx:webserver 查看镜像信息。
    
#####用Dockerfile构建镜像
>Dockerfile使用基本的基于DSL语法的指令来构建一个Docker镜像，之后使用docker build命令基于该Dockerfile中的指令构建一个新的镜像

大致流程：
1.  docker从基础镜像运行一个容器
2. 执行一条指令，对容器做出修改
3. 执行类型docker commit 的操作，提交一个新的镜像层
4. docker在基于刚提交的镜像运行一个新容器
5. 执行dockerfile的下一条指令，知道所有指令都执行完毕

编写好Dockerfile
执行dockerbuild

1. 构建镜像时设置标签

    docker build -t 'justudy/demo:v1' .
2. 从git仓库构建docker镜像

     docker build -t 'justudy/demo:v1' git@github.com:justudy/demo


####Dockerfile指令
1. CMD

    CMD指令用于指定一个容器启动时要运行的命令
    `CMD ["/bin/bash", "-1"]`
    在Dockerfile中只能指定一条CMD命令

2. ENTRYPOINT

    `ENTRYPOINT ["/usr/sbin/nginx", "-g", "daemon off;"]`
    
    docker run 后面带的命令会作为参数传递给ENTRYPOINT
3. WORKDIR

    从镜像创建一个新容器时，在容器内部设置一个工作目录。CMD和ENTRYPOINT指定的程序会在这个目录下执行。

    ```
    WORKDIR /opt/webapp/db
    RUN bundle install
    WORKDIR /opt/webapp
    ENTRYPOINT ["rackup"]
    ```
    
    docker run -w  可指定工作目录
4. ENV

    ENV指令用来在镜像构建过程中设置环境变量
    `ENV RVM_PATH /HOME/RVM`
    
    ```
    ENV TARGET_DIR /opt/app
    WORKDIR $TARGET_DIR
    ```    
    docker run -e "TARGET_DIR=/opt/app"   -e传递环境变量
    
5. USER

    USER指定用来指定该镜像以什么样的用户去执行
    
    `USER nginx`
    
    docker run -u 命令指定用户
    
6. VOLUME

    用来向基于镜像创建的容器添加卷，一个卷式可以存在于一个或者多个容器内的特定目录。
    `VOLUME ["/opt/project"]`
 
 7. ADD

    用来讲构建环境下的文件和目录复制到镜像中
    
    `ADD _linux/var/spool/cron/crontabs/root /var/spool/cron/crontabs/root`
        
8. COPY

    COPY不会提取解压文件
    `COPY ./composer.json /app/`
9. ONBUILD
    为镜像添加触发器，当一个镜像被用作其他镜像的基础镜像时，该镜像的触发器将会执行
    
```
ONBUILD ADD . /app/src
ONBUILD RUN cd /app/src && make
```
####讲镜像推送到Docker Hub

镜像构建完毕，将他上传到docker hub。
`docker push youruser/yourimage`

