---
title: "Compose"
date: 2020-01-04T21:53:40+08:00
categories: ["Container"]
tags: ["docker"]
keywords: []
author: "Canux"
draft: false
---

# docker-compose

<https://github.com/docker/compose>

通过一个yaml文件来管理容器中的服务，包括网络和存储。

安装:

    https://docs.docker.com/compose/install/
    $ sudo curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
    $ sudo chmod +x /usr/local/bin/docker-compose

# docker-compose命令

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

# docker-compose.yml

compose文件

    version: "3.6"
    services:
      mongo:
        image: mongo:latest
        hostname: hostname

        networks:
        - mynetwork
        // 只有自定义网络可以指定静态IP
        networks:
          mynetwork:
            ipv4_address: 172.19.0.100

        volumes: // short syntax
        - myvolume:/container/dir
        volumes: // long syntax
        - type: valume/bind/tmpfs
          source: 
          target:
          read_only:
          bind:
          volume:
          tmpfs:
          consistency:
        volumes:
        - "/path/to/file:/path/to/file 挂载文件

        logging:
          driver: syslog
          options:
            syslog-address: "tcp://192.168.0.42:123"

        ports:  // long syntax
        - target: 80
          published: 8080
          mode: host
          protocol: tcp/udp
        ports: // short syntax
          - 80:80
          - 1234:1234/udp

        environment:
          RABBITMQ_DEFAULT_USER: sandbox
          RABBITMQ_DEFAULT_PASS: password
        environment:
          - RABBITMQ_DEFAULT_USER=sandbox
          - RABBITMQ_DEFAULT_PASS=password

        <https://docs.docker.com/compose/compose-file/#env_file>
        # When you set the same environment variable in multiple files, here’s the priority used by Compose to choose which value to use:
        # Compose file
        # Shell environment variables
        # Environment file
        # Dockerfile
        # Variable is not defined
        env_file:

        depends_on:
          - service-name

        command: ["./wait-for-it.sh", "db:5432", "--", "python", "app.py"]

        // 解析为: "line1 line2\n",  会自动加换行符.
        command: >
        line1
        line2

        // 解析为: "line1 line2", 没有换行符.
        command: >-
        line1
        line2

        // 在compose v3 中针对非swarm模式的container做资源限制等操作.
        // --compatibility If set, Compose will attempt to convert keys in v3 files to their non-Swarm equivalent
        $ docker-compose --compatibility up -d
        // --compatibility 支持下面三种key:
        replicas:
        restart_policy:
          condition:
          max_attempts:
        resources:
          limits:
            cpus: '0.5'
            memory: 1G
          reservations:
            cpus: '0.25'
            memory: 20M

        privileged: true
        devices:
          - /dev/vboxdrv:/dev/vboxdrv

        deploy: // for swarm
        # 下列选项不能用于swarm stack部署.
        sysctls(19.03+)
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
        userns_mode
        ulimits
        cap_add
        cap_drop
    ​
    // (推荐)使用已经创建好的网络
    // 通过命令行或者api 创建网络
    networks:
      mynetwork:
        external: true
        name: lan0 // 通过docker network ls 查看名字
    ​
    // 创建bridge网络
    // 会在网络名字自动加namespace
    networks:
      mynetwork:
        driver: bridge
        driver_opts:
          com.docker.network.bridge.name: lan0
        ipam:
          driver: default
          config:
            - subnet: 192.168.1.0/24

    // 创建overlay网络
    // 会在网络名字自动加namespace
    networks:
      ol0:
        driver: overlay
        attachable: true
        driver_opts:
          com.docker.network.bridge.name: ol0
        ipam:
          driver: default
          config:
          - subnet: 172.12.0.0/16

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

# Kompose

<https://github.com/kubernetes/kompose>

install:

    $ curl -L https://github.com/kubernetes/kompose/releases/download/v1.19.0/kompose-linux-amd64 -o kompose

    $ chmod +x kompose
    $ sudo mv ./kompose /usr/local/bin/kompose
