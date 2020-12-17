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

docker-cli -> dockerd -> containerd -> shim -> runc

containerd是容器运行时管理引擎.

shim用于管理容器生命周期.

Docker包括三个基本概念:

* 仓库repository,集中存放镜像文件的场所,docker hub/store是最大的公开仓库．
* 镜像image, 镜像是一个文件系统.
* 容器container, 容器是镜像的运行的实例．

修改docker存储路径:

    $ service docker stop
    $ mv /var/lib/docker /opt/ssd/docker
    $ ln -s /opt/ssd/docker /var/lib/docker
    $ service docker start

Install:

    windows:
    <https://docs.docker.com/docker-for-windows/install/>

    linux:
    <https://docs.docker.com/engine/install/ubuntu/>

# 配置

<https://docs.docker.com/engine/reference/commandline/dockerd/#daemon-configuration-file>

docker配置文件:

    /etc/docker/daemon.json
    /lib/systemd/system/docker.service

    {
        // debug
        "debug": true,
        
        "data-root"： "/var/lib/docker",
        
        "features": {
            "buildkit": true
        },

        //容器访问外网:
        ip-forward=true 会设置 net.ipv4.ip_forward=1, 才能访问外网
        // 容器之间访问:
        icc=true, 
        iptables=true  会修改iptables的forward策略为accept,

        // 修改默认docker0
        "bridge":
        "bip": "10.0.0.1/16"  // subnet + gateway
        "fixed-cidr": "10.41.0.0/24" // iprange
        "fixed-cidr-v6": "",
        "mtu": 1500
        "default-gateway":
    	"default-gateway-v6": "",

        // 修改默认dns
        "dns" : [
            "114.114.114.114",
            "8.8.8.8"
        ]
	    "dns-opts": [],
	    "dns-search": [],

        // ipv6
        "ipv6": true

        // private registry
      	"insecure-registries": [],

        // 修改registry
        "registry-mirrors": [
            "https://registry.docker-cn.com",
            "https://z4yd270h.mirror.aliyuncs.com",
            "http://f1361db2.m.daocloud.io",
            "https://docker.mirrors.ustc.edu.cn"
        ]

        "hosts": [],
        "log-level": "",
        "tls": true,
        "tlsverify": true,
        "tlscacert": "",
        "tlscert": "",
        "tlskey": "",
    }

proxy for pull image from google(gcr):

<https://docs.docker.com/config/daemon/systemd/#httphttps-proxy>

***

# docker命令

system:

    $ docker system df	// Show docker disk usage
    $ docker system events	// Get real time events from the server
    $ docker system info	// Display system-wide information
    $ docker system prune	// Remove unused data

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
    docker build --no-cache // 不使用缓存数据

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

    // cpu
    --cpus decimal                   Number of CPUs
    -c, --cpu-shares int             CPU shares (relative weight)
    --cpuset-cpus string             CPUs in which to allow execution (0-3, 0,1)
    --cpuset-mems string             MEMs in which to allow execution (0-3, 0,1)
    --cpu-period
    --cpu-quota

    // memory
    -m, --memory bytes               Memory limit
    --memory-reservation bytes       Memory soft limit
    --memory-swap bytes              Swap limit equal to memory plus swap: '-1' to enable unlimited swap
    --memory-swappiness int          Tune container memory swappiness (0 to 100) (default -1)
    --oom-kill-disable
    --oom-score-adj
    --kernel-memory

    // io
    --blkio-weight
    --blkio-weight-device
    --device-read-bps
    --device-write-bps
    --device-read-iops
    --device-write-iops

    // security
    // https://docs.docker.com/engine/security/apparmor/
    // https://docs.docker.com/engine/security/seccomp/
    // https://docs.docker.com/engine/security/userns-remap/
    // https://docs.docker.com/engine/security/rootless/
    privileged
    sysctl
    ulimit
    user
    userns
    security_ops
    cgroup_parent
    cap_add
    cap_drop

    # 在运行的container中执行命令
    docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
    docker exec -d CONTAINER ... // 在后台运行
    docker exec -it CONTAINER /bin/bash ... // 进入命令行

    # metrics
    docker stats

    # resource
    docker top

log:

    // 查看log driver
    // 默认driver是 json-file, 不同的driver，option不同
    // driver是json-file, journald, 通过docker logs docker-compose logs 才能看到log
    $ docker inspect -f '{{.HostConfig.LogConfig.Type}}' <ID>

    docker run -it --log-driver <driver> --log-opt mode=blocking --log-opt max-buffer-size=4m alpine ash  
    // --log-driver json-file
    // --log-opt mode blocking/non-blocking
    // --log-opt max-buffer-size 

其它命令

    # 在container和host之前拷贝文件和目录
    docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH|-
    docker cp [OPTIONS] SRC_PATH|- CONTAINER:DEST_PATH

    # 查看容器的日志
    docker logs [OPTIONS] CONTAINER
    docker diff CONTAINER
    docker history [OPTIONS] IMAGE

    docker system prune

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

secret

    // 保存敏感数据
    $ docker secret
    
    docker secret ls
    docker secret inspect <ID/name>
    docker secret rm <id/name>
    
    docker secret create --help

config

    // 保存非敏感数据
    $ docker config
    
    docker config ls
    docker config inspect <id/name>
    docker config rm <id/name>
    
    docker config create --help

***

# Dockerfile

每个命令都会创建一个layer,尽可能合并相同的命令。

ADD

    # add可以是远程文件，不推荐使用
    ADD [--chown=<user>:<group>] <src>... <dest>
    ADD [--chown=<user>:<group>] ["<src>",... "<dest>"]

COPY

    # 只能操作本地文件, 目标路径不需要创建，不存在会自动创建.
    COPY [--chown=<user>:<group>] <src>... <dest>
    COPY [--chown=<user>:<group>] ["<src>",... "<dest>"]
    COPY file /path/to/file  相当于 COPY file /path
    # 不会创建目录
    COPY folder /path 相当于 COPY folder/* /path/
    # 需要手动指定
    COPY folder  /path/to/folder 

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

    # 指定后的操作都以该目录为当前目录，目录不存在会自动创建
    WORKDIR /path/to/workdir

RUN

    # 在容器构建过程中运行
    RUN <command>  # shell 格式
    RUN ["executable", "param1", "param2"]   # exec 格式

    RUN set -ex \
    && cmd1 \
    && cmd2......

CMD

    # 在容器运行过程中运行, 可以被覆盖
    CMD ["executable", "param1", "param2"]  # exec格式
    CMD command param1 param2   # shell 格式,通过/bin/bash 或 /bin/sh 执行.
    // 同时有cmd和entrypoint,cmd只是entrypoint的参数.
    CMD ["param2", "param2"]   # entrypoint的参数

ENTRYPOINT

    # 和cmd类似,指定容器运行过程中的执行命令
    ENTRYPOINT ["executable", "param1", "param2"] (exec form, preferred)
    ENTRYPOINT command param1 param2

ARG

    # 指定构建环境的变量
    ARG <name>[=<default value>]

ONBUILD

    ONBUILD ...

stage:

多级构建，一般0级用来编译源代码，并且组织目录结构。
下一级直接将编译好的或者组织好的目录结构copy到指定位置。
这样最终的image不会含有源代码的overlay.
也不会有临时目录结构的overlay.

    # 不指定stage name, 默认就是数字
    COPY --from=0 /path/folder /path/folder
    COPY --from=stage1 /path/folder /path/folder 
    docker build --target stage1 -t docker:latest . 

.dockerignore 

    # comment
    /path/folder
    path/folder
    */tmp*
    */*/tmp*
    tmp?
    *.md
    !README.md  

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

***

# OOM

<https://docs.docker.com/config/containers/resource_constraints/>

<https://github.com/docker/compose/issues/4513>

swarm mode:

swarm模式使用compose format 3来限制资源.

<https://docs.docker.com/compose/compose-file/#resources>

non swarm mode:

非swarm模式可以用compose format 2 来做资源限制.

<https://docs.docker.com/compose/compose-file/compose-file-v2/#cpu-and-other-resources>