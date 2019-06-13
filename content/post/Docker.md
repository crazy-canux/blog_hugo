---
title: "Docker"
date: 2018-04-05T22:02:31
categories: ["Container"]
tags: ["docker"]
keywords: []
author: "Canux"
draft: false
---

# Docker

<https://store.docker.com/>

<https://hub.docker.com/>

<https://github.com/docker>

<https://github.com/moby/moby>

Docker是一个容器引擎, 分为社区版CE, 和企业版EE, Docker不是虚拟机, 也不依赖虚拟化技术．

Docker包括三个基本概念:

* 仓库repository,集中存放镜像文件的场所,docker hub/store是最大的公开仓库．
* 镜像image, 镜像是一个文件系统.
* 容器container, 容器是镜像的运行的实例．

修改默认register:

    # http://7bd09afa.m.daocloud.io
    # https://docker.mirrors.ustc.edu.cn
    # https://registry.docker-cn.com
    $ sudo vim /etc/docker/daemon.json
    {
        "registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"]
    }

禁止docker修改iptables:

    $ sudo vim /lib/systemd/system/docker.service
    > ExecStart=/usr/bin/dockerd -H fd:// --iptables=false

Install:

    windows:
    <https://docs.docker.com/docker-for-windows/install/>

    linux:
    <https://docs.docker.com/install/linux/docker-ce/ubuntu/>

***

# docker命令

image管理:

    $ docker image COMMAND

    > 查看本地镜像:
    $ docker image ls
    $ docker image -a

    > 根据创建dockerfile，创建新的images
    $ docker image build

    > 创建tag
    $ docker image tag

    > 删除image
    $ docker image rm <IMAGE ID>
    $ docker rmi <IMAGE ID>
    > 删除所有image
    $ docker rmi $(docker images -a -q)

    > 清理所有临时images
    $ docker image prune

container管理

    $docker container COMMAND

    # 列出container:
    $ docker container ls
    $ docker ps -a  # 默认只显示running状态的

    # 运行image,产生一个container:
    $ docker container run <IMAGE ID>/<REPOSITORY> [COMMAND] [ARGS]

    # 在container中执行命令
    $ docker container exec [OPTIONS] <CONTAINER> COMMAND [ARG...]

    # 创建container但不启动
    $ docker container create --name <name> <CONTAINER>

    # 启动container:
    $ docker container start/restart <CONTAINER>

    # 停止container:
    $ docker container stop <CONTAINER>

    # 删除container：
    $ docker container rm <CONTAINER>
    $ docker rm <CONTAINER>
    # 删除所有容器
    $ docker rm $(docker ps -a -q) 

    # 清理停止的container
    $ docker container prune

制作镜像

    # 根据Dockerfile 构建image
    docker build [OPTIONS] PATH | URL | -
    docker build .  // 默认就是当前目录的Dockerfile
    docker build -t <repo>/<name>:<tag> .  // 创建tag
    docker build -f /path/to/mydockerfile . // 也可以指定其它路径的其它文件
    docker build --target <stage> . // 指定阶段构建.
    docker build ... --network=host // 使用host网络构建.

    # 把image导出到tar包
    # 既可以从image也可以从container导出。
    # 从container导出不包含运行后的修改，只导出原始镜像。
    $ docker save -o name.tgz <repo1>:<tag1> <repo2>:<tag2> ...

    # 从stdin或文件加载image
    docker load [OPTIONS]
    docker load < name.tar.gz
    docker load --input/-i name.tar

    # 把container导出到tar包，从container导出镜像。
    # 包括container启动后的修改。
    docker export -o name.tar [container]

    # 从container导出的包加载成镜像
    docker import name.tar [repo]:[tag]

    # 根据container的修改创建新的image
    docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
    docker commit -a "author" -c "Dockerfile instruction" -m "commit message" CONTAINER [REPOSITORY[:TAG]]

    # 创建新的tag
    docker tag <old> <new>

运行容器

    # 创建一个新的container并运行命令
    docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
    docker run --name [NAME] IMAGE // 运行容器并命名
    docker run -d IMAGE // 后台运行
    docker run -it IMAGE /bin/bash // 交互模式启动容器
    docker run -P IMAGE // 默认将容器的8０端口映射到主机的随机端口
    docker run -p [host:port]:[containerPort] // 指定映射端口
    --add-host # 相当于修改容器的/etc/hosts,但是容器重启后不会消失
    -h/--hostname # 修改容器的/etc/hostname
    -c, --cpu-shares int             CPU shares (relative weight)
    --cpus decimal                   Number of CPUs
    --cpuset-cpus string             CPUs in which to allow execution (0-3, 0,1)
    --cpuset-mems string             MEMs in which to allow execution (0-3, 0,1)
    -m, --memory bytes               Memory limit
    --memory-reservation bytes       Memory soft limit
    --memory-swap bytes              Swap limit equal to memory plus swap: '-1' to enable unlimited swap
    --memory-swappiness int          Tune container memory swappiness (0 to 100) (default -1)

    # 在运行的container中执行命令
    docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
    docker exec -d CONTAINER ... // 在后台运行
    docker exec -it CONTAINER /bin/bash ... // 进入命令行

其它命令

    # 在container和host之前拷贝文件和目录
    docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH|-
    docker cp [OPTIONS] SRC_PATH|- CONTAINER:DEST_PATH

    # 查看容器的日志
    docker logs [OPTIONS] CONTAINER
    docker diff CONTAINER
    docker history [OPTIONS] IMAGE

register使用

    # 登陆到docker hub 或其他register
    docker login
    docker login -u/--username <user> -p/--password <password>

    # 从docker hub/store查找images
    $ docker search [OPTIONS] TERM
    $ docker search

    # 从registry获取repository/images到/var/lib/docker：
    $ docker pull [OPTIONS] NAME[:TAG|@DIGEST]
    $ docker pull
    $ docker pull ubuntu # 默认下载所有tag
    $ docker pull ubuntu:14.04

    # 从中国站点下载
    $ docker pull registry.docker-cn.com/library/ubuntu:16.04

    # 推送到docker hub
    $ docker push

***

# Dockerfile

ADD

    # add可以是远程文件，不推荐使用
    ADD [--chown=<user>:<group>] <src>... <dest>
    ADD [--chown=<user>:<group>] ["<src>",... "<dest>"]

COPY

    # 只能操作本地文件
    COPY [--chown=<user>:<group>] <src>... <dest>
    COPY [--chown=<user>:<group>] ["<src>",... "<dest>"]

ENV

    # 指定容器中的环境变量
    ENV <key> <value>
    ENV <key>=<value> ...

EXPOSE

    # 指定容器需要映射到主机的端口
    EXPOSE <port> [<port>/<protocol>...]

FROM

    FROM <image> [AS <name>]
    FROM <image>[:<tag>] [AS <name>]
    FROM <image>[@<digest>] [AS <name>]

LABEL

    LABEL <key>=<value> <key>=<value> <key>=<value> ...

STOPSIGNAL

    STOPSIGNAL signal

USER

    USER <user>[:<group>]
    USER <UID>[:<GID>]

VOLUME

    # 指定挂载点做数据持久化
    VOLUME ["/data"]

WORKDIR

    # 对run/cmd/entrypoint有效的工作目录
    WORKDIR /path/to/workdir

RUN

    # 在容器构建过程中运行
    RUN <command>  # shell 格式
    RUN ["executable", "param1", "param2"]   # exec 格式

CMD

    # 在容器运行过程中运行, 可以被覆盖
    CMD ["executable", "param1", "param2"]  # exec格式
    CMD command param1 param2   # shell 格式
    // 同时有cmd和entrypoint,cmd只是entrypoint的参数.
    CMD ["param2", "param2"]   # entrypoint的参数

ENTRYPOINT

    # 和cmd类似,指定容器运行过程中的执行命令
    ENTRYPOINT ["executable", "param1", "param2"] (exec form, preferred)
    ENTRYPOINT command param1 param2

ARG

    ARG <name>[=<default value>]

ONBUILD

    ONBUILD ...

stage:

    COPY --from=stage1 / .
    docker build --target stage1 -t docker:latest . 

***

# docker-compose

<https://github.com/docker/compose>

通过一个yaml文件来管理容器中的服务，包括网络和存储。

安装:

    https://docs.docker.com/compose/install/
    $ sudo curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
    $ sudo chmod +x /usr/local/bin/docker-compose

## docker-compose命令

    docker-compose [-f <arg>...] [options] [COMMAND] [ARGS...]
    -f/--file
    -p/--project-name # 默认目录名
    -H/--host

    # 拉取compose文件中指定的镜像
    $ docker-compose -f service.yml pull

    # 根据docker-compose.yml把stack打包成一个Distributed Application Bundles文件.
    $ docker-compose bundle -o <project name>.dab

    $ docker-compose start [servoce...]
    $ docker-compose stop [service...]
    $ docker-compose restart [service...]
    $ docker-compose up -d [service...]
    $ docker-compose down -v

    $ docker-compose logs -f

## docker-compose.yml

compose文件

    version: "3.6"
    services:
      mongo:
        image: mongo:latest
        hostname: hostname
        networks:
        - mynetwork
        volumes:
        - myvolume
        deploy: // for swarm
        ports:
          - 80:80
          - 1234:1234/udp
        environment:
          RABBITMQ_DEFAULT_USER: sandbox
          RABBITMQ_DEFAULT_PASS: password
        environment:
          - RABBITMQ_DEFAULT_USER=sandbox
          - RABBITMQ_DEFAULT_PASS=password
        depends_on:
          - service-name

        # 下列选项不能用于swarm stack部署.
        build
        cgroup_parent
        container_name
        devices
        tmpfs
        external_links
        links
        network_mode
        restart
        security_opt
        sysctls
        userns_mode
    ​
    networks:
      mynetwork:
        external:
          name: lan0
    ​
    networks:
      mynetwork:
        driver: bridge
        driver_opts:
          com.docker.network.bridge.name: lan0
        ipam:
          driver: default
          config:
            - subnet: 192.168.1.0/24


    # compose文件中用到的变量
    .Service.ID Service ID 
    .Service.Name Service name 
    .Service.Labels Service labels 
    .Node.ID Node ID 
    .Node.Hostname Node Hostname 
    .Task.ID Task ID 
    .Task.Name Task name 
    .Task.Slot Task slot 

***

# docker-machine

<https://github.com/docker/machine>

在本地安装docker和docker-machine，然后就可以从本机安装或管理远程机器上的docker.

需要添加ssh的无密码登陆:

    ssh-copy-id -i ~/.ssh/id_rsa.pub user@remote-ip

安装:

    https://docs.docker.com/machine/install-machine/$ base=https://github.com/docker/machine/releases/download/v0.16.0
    && curl -L $base/docker-machine-$(uname -s)-$(uname -m) >/tmp/docker-machine
    && sudo install /tmp/docker-machine /usr/local/bin/docker-machine

## docker-machine命令

    docker-machine create -d generic \
    --generic-ip-address=remote-ip \
    --generic-ssh-user=user \
    --generic-ssh-key ~/.ssh/id_rsa \
    node1
    docker-machine ls