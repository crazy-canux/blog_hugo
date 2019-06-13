---
title: "Swarm"
date: 2018-04-05T22:02:31
categories: ["Container"]
tags: ["swarm"]
keywords: []
author: "Canux"
draft: false
---

# Swarm

docker swarm 是 docker内置的容器编排工具。

从docker1.12开始swarm内置于docker engine.

swarm mode具有内置kv存储，服务发现，负载均衡，陆游网格，动态伸缩，滚动更新，安全传输等功能。

部署服务之前所有node上都需要相关的images。

# swarm命令

创建集群

    docker swarm init
    --advertise-addr <ip> 多网卡情况下指定manager的ip
    ​
    docker swarm join --token <token> <host:port>
    ​
    # 查看token
    docker swarm join-token manager   获取添加manager命令
    docker swarm join-token worker   获取添加worker命令
    ​
    docker swarm leave -f/--force
    ​
    docker swarm update

管理节点

    docker node ls
    docker node ls --format "{{.Hostname}}"
    ​
    docker node rm
    ​
    docker node inspect
    ​
    # 查看node上运行的tasks/container
    docker node ps -f/--filter NODE
    ​
    # 添加label, node.labels.role=api
    docker node update --label-add role=api node1
    # 删除label
    docker node update --label-rm role node1
    ​
    # 活跃节点
    docker node update --availability active node1
    # 指定该节点满载,不再分派任务,关闭已有任务并重新分派.
    docker node update --availability drain node1
    # 已有任务继续运行,不分配新任务.
    docker node update --availability pause node1

service

    # 相当于docker-compose.yml里面的service.
    docker service ls # 列出所有service
    ​
    docker service rm SERVICE
    ​
    docker service inspect SERVICE
    ​
    # 查看service的log
    docker service logs -f SERVICE
    ​
    # 查看service的状态,在哪些node上运行,运行状态等
    docker service ps SERVICE
    ​
    docker service update
    docker service update --image <url:tag> # 根据镜像更新服务
    ​
    docker service scale
    ​
    docker service rollback
    ​
    docker service create
    --constraint node.id/node.hostname/node.role/node.labels/engine.labels
    --env/-e
    --label
    --limit-cpu
    --limit-memory
    --replicas
    --restart-condition
    --user/--group
    --mode global/replicated
    --endpoint-mode vip/dnsrr

stack

    # stack = n*service
    # service = n*task(container)
    docker stack ls # 列出所有stack
    ​
    # 查看stack的service
    docker stack services
    ​
    # 查看stack的task/container
    docker stack ps STACK
    ​
    # 根据docker-compose.yml部署应用
    docker stack deploy -c/--compose-file <docker-compose.yml> STACK
    docker stack deploy --bundle-file <DAB> STACK
    ​
    docker stack rm STACK

***

# swarm compose

通过compose文件部署服务.

    deploy:
      mode: global # 部署到匹配的全部node.
    ​
      mode: replicated
      reoplicas: 3
    ​
      # global和replicated都可以用placement.
      placement:
        constraints:
          - node.role == manager
        preferences:
          - spreed:
    ​
      resources:
        limits:
          cpus: '0.5'
          memory: 1G
        reservations:
          cpus: '0.25'
          memory: 20M
    ​
      restart_policy:
        condition: on-failure
        delay: 5s
        max_attempts: 3
        window: 10s
    ​
      update_config:
    ​
      rollback_config:

      // 默认是vip,支持route mesh, 自动负载均衡和服务发现.
      // dnsrr只能用port->mode=host.
      endpoint_mode: vip
      ports:
      - target: 80
        published: 8080
        mode: ingress
        protocol: tcp/udp
      - 8080:80/tcp
      endpoint_mode: dnsrr
      ports:
      - target: 80
        published: 8080
        mode: host
        protocol: tcp/udp

***

# swarm network

container内部有eth0和eth1两个虚拟网卡。

eth0(overlay) 通过4789/udp跨主机访问其他container.

eth1(docker_gwbridge)通过tcp访问host和外网.

container之间通过service-name调用服务，
replicated类型service通过service-name访问，global类型通过service-name和hostname都能访问。

外部通过port访问服务，manager通过ingress网络做端口转发和route mesh自动做负载均衡。

endpoint_mode:

* vip: 通过vip这个虚拟ip对外访问，提供负载均衡，不暴露具体的container的ip.
* dnssr: DNS round-robin, 为每个服务设置dns,连接到其中一个具体的contaier的ip.

***

# swarm scheduler

filter过滤器可以实现特定的容器运行在特定的node上, swarm支持３种策略和6个过滤器.

swarm strategy:

* spread： 默认策略,配置相同的情况下选择容器数量最少的node
* binpack： 尽可能将容器放到一台node上运行。
* random： 直接随机分配

swarm node filters:

* constraint
* health
* containerslots

swarm container-configuration filters:

* affinity
* dependency
* port
