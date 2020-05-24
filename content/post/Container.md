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

# tools

## dumb-init

管理pid=1的进程的子进程:

<https://github.com/Yelp/dumb-init>

## baseimage-docker

处理container中运行多个进程的问题:

<https://github.com/phusion/baseimage-docker>

## watchtower

根据registry中的更新自动更新 container:

<https://github.com/containrrr/watchtower/>

## hadolint

dockerfile 语法检查:

<https://github.com/hadolint/hadolint>

***

# CLI

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