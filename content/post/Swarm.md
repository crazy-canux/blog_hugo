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

直接在manager上通过compose文件运行服务，通过service-name调用服务，所有node上都需要相关的images。

# swarm命令

创建集群

    docker swarm init
    --advertise-addr <ip> 指定manager的ip
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
    ​
    docker node update --availability pause node1

service

    相当于docker-compose.yml里面的service.docker service ls # 列出所有service
    ​
    docker service rm SERVICE
    ​
    docker service inspect SERVICE
    ​
    # 查看service的log
    docker service logs SERVICE
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
    compose file

    deploy:
    ​
      mode: global # 部署到匹配的全部node.
    ​
      mode: replicated
      reoplicas: 3
    ​
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
    ​
stack

    > stack = n*service
    > service = n*task(container)
    docker stack ls # 列出所有stack
    ​
    # 查看stack的service
    docker stack services
    ​
    查看stack的task/container
    docker stack ps STACK
    ​
    根据docker-compose.yml部署应用
    docker stack deploy -c/--compose-file <docker-compose.yml> STACK
    docker stack deploy --bundle-file <DAB> STACK
    ​
    docker stack rm STACK

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
