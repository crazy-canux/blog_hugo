---
title: "Swarm CNI"
date: 2018-04-05T22:02:31
categories: ["Container"]
tags: ["swarm"]
keywords: []
author: "Canux"
draft: false
---

# CNI

CNI: Container Network Intarface

## 单台host上的container通信

* none
* host
* bridge

## 多台host之间的container通信

* overlay
* macvlan

### 提供overlay/macvlan的网络服务

vxlan encapsulated:

* canal
* flannel
* weave

bgp unencapsulated:

* calico
* romana
* cilium
* kube-router

***

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

***

# bridge网络

bridge网络不能跨主机通信(单网卡情况下), node1上的container不能通过container-hostname/ip访问node2上的container.

主要用于container访问host并通过host访问外部网络，container能通过ip访问host和局域网中的其他node,或者通过node访问外网。

host或局域网中的其它机器能通过container-ip(bridge网络)访问container.

创建:

    $ docker network create -d bridge ... [name]

    $ docker network create --driver=bridge --gateway=192.168.1.1 --subnet=192.168.1.0/24 --opt com.docker.network.bridge.name=br0 br0

    // 定制docker_gwbridge网络
    $ docker network create --subnet 172.26.0.0/16 --ip-range 172.26.0.0/16 --gateway 172.26.0.1 \ 
    --opt com.docker.network.bridge.name=docker_gwbridge \
    --opt com.docker.network.bridge.enable_icc=true \ 
    --opt com.docker.network.bridge.enable_ip_masquerade=true \ 
    docker_gwbridge

docker0:

    // dockerd 自动的默认bridge网络，不推荐用于production.

    1. 删除docker0:

    // 停止
    $ ip link set dev docker0 down
    $ ifconfig docker0 down

    // 删除
    $ ip link delete docker0
    $ brctl delbr docker0

    2. 修改默认网络:
    $ vim /etc/docker/daemon.json
    {
        "bip": "192.168.1.5/24",
        "fixed-cidr": "192.168.1.5/25",
        "fixed-cidr-v6": "2001:db8::/64",
        "mtu": 1500,
        "default-gateway": "10.20.1.1",
        "default-gateway-v6": "2001:db8:abcd::89",
        "dns": ["10.20.1.2","10.20.1.3"]
    }
    $ service docker restart

# overlay网络

overlay网络可以实现容器之间的跨主机通信.

container通过overlay网络实现通信.container能通过service-name/container-ip访问其它container。

局域网中的node 既不能通过container-servicename也不能通过container-ip(overlay的ip)访问container, 也就是说外部服务只能通过expose port来访问container.

创建:

    $ docker network create -d overlay ... [name]

    $ docker network create --attachable --driver=overlay --gateway=172.27.0.1 --subnet=172.27.0.0/24 --ip-range=172.27.0.0/24 --opt com.docker.network.bridge.name=ol0 ol0

# macvlan

macvlan不仅支持在interface上创建，还支持sub-interface(vlan).

    ip link set eth1 promisc on  |  
    ifconfig eth1 promisc 

    docker network create -d macvlan --subnet=192.168.100.0/24 --gateway=192.168.100.1 -o parent=eth1 lan0

***