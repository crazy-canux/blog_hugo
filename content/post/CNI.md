---
title: "CNI"
date: 2018-04-05T22:02:31
categories: ["Container"]
tags: ["network"]
keywords: []
author: "Canux"
draft: false
---

# CNI

CNI: Container Network Intarface

单台host上的container通信:

* none
* host
* bridge

多台host之间的container通信:

* overlay
* macvlan

提供overlay/macvlan的网络服务:

* flannel
* cilium
* kube-router
* calico

# Docker网络管理

查看:

    $ docker network ls # 查看所有网络

默认支持的三种模式: 

    # 默认启动的容器都是桥接(docker0)，重启后容器的ip就变了。
    docker run --network bridge  ...
    docker run --network host ... # 容器和主机使用相同的ip
    docker run --network none ... # 容器不会分配局域网的ip

创建:

    docker network create -d <driver> ... [name]
    -d/--driver # 默认是bridge, 可选overlay/macvlan
    --subnet  # CIDR格式
    --gateway
    --ip-range

使用:

    $ docker network connect [OPTIONS] NETWORK CONTAINER
    $ docker network disconnect [OPTIONS] NETWORK CONTAINER
    $ docker run --network [name] --name [container-name] [image:tag]

    # 使用自定义bridge网络并指定IP:
    docker run --network [name] --ip [ip] ...

opt可用的参数:

    com.docker.network.bridge.name # bridge名字
    com.docker.network.bridge.enable_ip_masquerade # iptables:nat, 容器访问外网.
    com.docker.network.bridge.enable_icc # iptables:filter, 同一网段容器相互访问.
    com.docker.network.bridge.host_binding_ipv4
    com.docker.network.driver.mtu

# bridge网络

bridge网络不能跨主机通信(单网卡情况下), node1上的container不能通过container-hostname找到node2上的container.

创建:

    $ docker network create -d bridge ... [name]

    $ docker network create --driver=bridge --gateway=192.168.1.1 --subnet=192.168.1.0/24 --opt com.docker.network.bridge.name=br0 br0

# overlay网络

overlay网络可以跨主机通信，swarm默认使用overlay网络, container之间可以通过service-name通信.

container能通过ip访问局域网中的其它node, 但是不能通过hostname访问其它node. 局域网中的node 既不能通过container-servicename也不能通过container-hostname/ip访问container, 也就是说外部服务只能通过expose port来访问container.

创建:

    $ docker network create -d overlay ... [name]

    $ docker network create --attachable --driver=overlay --gateway=192.168.1.1 --subnet=192.168.1.0/24 --opt com.docker.network.bridge.name=br0 br0