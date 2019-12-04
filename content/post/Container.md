---
title: "Container"
date: 2018-04-05T22:02:31
categories: ["Container"]
tags: ["container"]
keywords: []
author: "Canux"
draft: false
---

# Container

OCI: Open Container Initiative.

CRI: Container Runtime Interface.

CNI: Container Network Interface.

CSI: Container Storage Interface.

# OCI

Open Container Initiative，也就是常说的OCI，是由多家公司共同成立的项目，并由linux基金会进行管理，致力于container runtime的标准的制定和runc的开发等工作.

是container的标准.

目前主要有两个标准文档：容器运行时标准 （runtime spec）和 容器镜像标准（image spec）

<https://www.opencontainers.org/>

## runc

docker(libcontainer)

runc支持OCI.

<https://github.com/opencontainers/runc>

## rkt

redhat(coreos)

<https://github.com/rkt/rkt>

***

# nsenter

nsenter - run program with namespaces of other processes

    $ nsenter [options] [program [arguments]]

    $ nsenter -a --all

    $ nsenter -n/--net <namespace> <command>
    $ nsenter --net=/var/run/docker/netns/lb_atz8r3bly ifconfig

    $ nsenter -m/--mount <namespace> <command>

***

# misc

get host ip(docker/docker_gwbridge) from container:

    ip route | awk '/default/ { print $3 }'