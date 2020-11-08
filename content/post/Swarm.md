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

swarm mode具有内置kv存储，服务发现，负载均衡，路由网格，动态伸缩，滚动更新，安全传输等功能。

swarm: 老版本的swarm, 需要kv store, 可以作为独立的container运行, 已废弃, 已经被docker swarm mode 代替.

<https://github.com/docker/swarm>

swarmkit:

<https://github.com/docker/swarmkit>

# swarm命令

创建集群

    docker swarm init
    --advertise-addr <ip> 多网卡情况下指定manager的ip
    
    docker swarm join --token <token> <host:port>
    
    # 查看token
    docker swarm join-token manager   获取添加manager命令
    docker swarm join-token worker   获取添加worker命令
    docker swarm join-token -q worker
    
    docker swarm leave -f/--force
    
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
    
    # 查看stack的service
    docker stack services <stack>
    
    # 查看stack的task/container
    docker stack ps <stack>

    docker stack rm STACK
    
    # 根据docker-compose.yml部署应用
    docker stack deploy -c/--compose-file <docker-compose.yml> STACK
    docker stack deploy --bundle-file <DAB> STACK

    # 默认所有node从docker hub pull, 如果是私有镜像，需要加参数
    # 需要在manager上docker login private-registry
    docker stack deploy --with-registry-auth -c test.yml test
    
    docker stack deploy --resolve-image (always|changed|never) ...
    
    docker stack deploy --orchestrator (swarm|kubernetes|all) ...

***

# swarm compose

通过compose文件部署服务.

    deploy:

      // 默认mode=replicated, replicas=1.
      mode: replicated
      replicas: 1

      // 部署到匹配的全部node.
      mode: global 
    ​
      # global和replicated都可以用placement.
      placement:
        preferences: // 只支持spreed.
          - spreed: node.labels.datacenter
        constraints:
          - node.role == manager
          - node.labels.role == "lable-name"
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
        condition: any(default)/on-failure/none
        delay: 0(default)/5s
        max_attempts: never give up(default)/3
        window: decide immediately(default)/10s
    ​
      update_config/rollback_config:
        parallelism: 0 (default 0 means all)
        delay: 10s (容器升级间隔时间)
        failure_action: pause(default)/continue/rollback
        monitor: 0s (更新完成后确认成功的时间)
        max_failure_ratio: 更新期间允许的失败率
        order: stop-first(default)/start-first

    deploy:
      // 默认endpoint_mode=vip, 支持route mesh, 自动负载均衡和服务发现.
      endpoint_mode: vip
    ports:
    - target: 80
      published: 8080
      // 默认mode=ingress, 也可以改为host.
      mode: ingress
      protocol: tcp/udp
    - 8080:80/tcp

    deploy:
      // dnsrr 模式
      endpoint_mode: dnsrr
      update_config:
        // 如果expose端口，不能start-first, 否则报错no suitable node (host-mode port already in use on 1 node
        order: stop-first 
    // 设置iptables规则，外部访问8080通过prerouting做dnat指定目的ip，通过forward转发给container的80.
    // iptables->nat->prerouting: 
    // DNAT tcp -- !docker_gwbridge * 0.0.0.0/0 0.0.0.0/0 tcp dpt:8080 to:172.18.0.16:80
    // iptables->filter->forward:
    // ACCEPT tcp -- enp5s0 * 0.0.0.0/0 0.0.0.0/0 multiport dports 80
    ports:
    - target: 80
      published: 8080
      // dnsrr只能用port->mode=host.
      mode: host
      protocol: tcp/udp

***

# swarm network

global模式container:

    eth0: overlay(user define overlay)
    eth1: ingress(swarm define ingress)
    eth2: docker_gwbridge(swarm define bridge)

replicate模式container:

    eth0: overlay(user define overlay)
    eth1: docker_gwbridge(swarm define bridge)

overlay: 通过4789/udp跨主机访问其他container, host不能访问overlay的ip，只有container之间通过container-servicename或者container-overlay的ip相互访问. 

overlay 问题: 通过overlay连接，默认15分钟timeout, 所以数据库建议用dnsrr模式; 如果用vip模式，需要修改内核网络参数:  

    $ sudo sysctl -w net.ipv4.tcp_keepalive_time=600 net.ipv4.tcp_keepalive_intvl=60 net.ipv4.tcp_keepalive_probes=3

vip模式就是访问的虚拟ip,replicated的service如果有多个container,通过servicename访问的就是同一个vip,通过vip解析到背后container的真实overlay-ip(自动负载均衡). 

dnsrr模式就是直接解析container的overlay-ip来访问,如果是replicated的service有多个container,每次访问的就是从dns列表中根据负载均衡算法拿到其中一个overlay-ip.

ingress network: 是一个特殊的 overlay 网络，用于服务节点间的负载均衡。当任何 Swarm 节点在发布的端口上接收到请求时，它将该请求交给一个名为 IPVS 的模块。IPVS 跟踪参与该服务的所有IP地址，选择其中的一个，并通过 ingress 网络将请求路由到它。

docker_gwbridge: host和container之间通过ip访问, container能访问host的物理网卡的ip和docker_gwbridge的ip, host也能访问container的docker_gwbridge的ip, 但是container之间不能访问bridge的ip.

修改默认的docker_gwbridge:

    // 对于已存在的，要先删除
    $ service docker stop
    $ sudo ip link set docker_gwbridge down
    $ sudo ip link del dev docker_gwbridge
    // 创建swarm之前创建好网络
    $ docker network create --subnet "172.18.0.0/16"  --ip-range “172.18.1.0/16” \
    --opt com.docker.network.bridge.name=docker_gwbridge \
    --opt com.docker.network.bridge.enable_icc=false \
    --opt com.docker.network.bridge.enable_ip_masquerade=true \
    docker_gwbridge
    // 创建swarm
    $ docker swarm init

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
