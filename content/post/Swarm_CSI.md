---
title: "Swarm CSI"
date: 2018-04-05T22:02:31
categories: ["Container"]
tags: ["swarm"]
keywords: []
author: "Canux"
draft: false
---

# CSI

CSI: Container Storage Interface

CSI提供容器的数据持久化服务.

容器管理数据的两种方式：

数据卷(Volumes)

挂载主机目录(bind mounts)

临时文件系统(tmpfs)

## 数据存储原理

如果container上目录不存在，docker会自动创建

如果container目录存在且有内容，会被host上的目录覆盖掉，但不会被删除.

## Volumes

如果host上目录不存在，docker会自动创建

volumes是被设计用来持久化数据的，它的生命周期独立于容器.数据卷通过docker volume命令管理的，位于/var/lib/docker/volumes/下面.

Docker不会在容器被删除后自动删除 数据卷，并且也不存在垃圾回收这样的机制来处理没有任何容器引用的 数据卷。

创建:

    $ docker volume create <volume-name>
    $ docker volume rm <volume-name>

使用：

    $ docker run -v/--volume myvolume:/var/lib/app ...
    $ docker run --mount source=myvolume,target=/var/lib/app ...

## Bind mount

如果host上目录不存在会报错，需要提前创建.

bind mount就是直接将host路径挂在到docker．

source和target都是文件，即可挂载单个文件.

使用:

    $ docker run -v/--volume /opt/app:/var/lib/app:ro ...
    $ docker run --mount type=bind,source=/opt/app,target=/var/lib/app,readonly ...

## tmpfs

tmpfs是临时文件系统，也叫内存文件系统，就是将数据存在内存上。

tmpfs只能用于linux, 多个容器也不能共享，容器停止数据就销毁。

使用--mount可以指定参数，使用--tmpfs不能指定参数。

使用:

    $ docker run --mount type=tmpfs,destination=/tmp/app,tmpfs_size=10G tmpfs_mode=1777 ...
    $ docker run --tmpfs  ...

***

# Docker管理volume

查看:

    docker volume ls # 查看所有卷

创建volume:

    docker volume create my-volume 
    # mountpoint: /var/lib/docker/volumes/my-volume/_data.
    docker run -v/--volume my-volume:/container/path 
    # src=/var/lib/docker/volumes/my-volume/_data, dest=/container/path

指定路径作为volume：

    docker run -v /host/path:/container/path ...
    # src=/host/path, dest=/container/path

默认随机路径，数据不能持久化：

    # src=/var/lib/docker/volumes/... /_data (on host)
    docker run -v /path ....
    docker run --mount ...
