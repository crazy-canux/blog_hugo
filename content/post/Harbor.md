---
title: "Harbor"
date: 2018-04-05T22:02:31
categories: ["Container"]
tags: ["harbor"]
keywords: []
author: "Canux"
draft: false
---
   
# Harbor

<https://github.com/goharbor/harbor>

Habor是由VMWare中国团队开源的容器镜像仓库, 用于存储和分发docker镜像的registry服务器.

安装步骤:

1. 下载并解压安装包, https://github.com/goharbor/harbor/releases
2. 配置harbor.cfg;

修改配置:

    vim harbor.yml

运行安装程序:

    ./install.sh --with-notary --with-clair --with-chartmuseum

修改web的port:

    $ vim /data/harbor/docker-compose.yml
    proxy:
      ports:
        - 8080:80 # 默认http是80
        - 4433:443 # 默认https是443
    $ vim /data/harbor/harbor.yml
    hostname = ip:port

管理harbor:

    # cd /data/harbor
    docker-compose down -v 　停止并删除container

    > 更新配置
    # ./prepare --with-notary --with-clair --with-chartmuseum
    # docker-compose -f ./docker-compose.yml -f ./docker-compose.notary.yml -f ./docker-compose.clair.yml -f ./docker-compose.chartmuseum.yml up -d

    docker-cmopose up -d 启动

# docker使用harbor

Deploy a plain HTTP registry:

    # vim /etc/docker/daemon.json
    {  "insecure-registries" : ["myregistrydomain.com:5000"]}

Use self-signed certificates:

    # cp your-ca /etc/docker/certs.d/harbor.domain.com/ca.crt
    # vim /lib/systemd/system/docker.service
    ExecStart=/usr/bin/dockerd --insecure-registry harbor.domain.com:port
    # systemctl daemon-reload
    # systemctl restart docker

update hosts on docker client:

    # vim /etc/hosts ip harbor.domain.com

create user account:

    https://<ip:port>
    
push images to harbor:

    $ docker login harbor.domain.com:port
    $ docker tag SOURCE_IMAGE[:TAG] harbor.domain.com:port/<project>/IMAGE[:TAG]
    $ docker push harbor.domain.com:port/<project>/IMAGE[:TAG]

pull images from harbor:

    $ docker login harbor.domain.com:port
    $ docker pull harbor.domain.com:port/<project>/IMAGE[:TAG]

k8s使用harbor:

<https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/>

# 修改存储路径

默认路径是/data/registry

停服务:

    $ docker-compose down -v

修改路径:

    $ mv /data/* /new/path
    $ vim /new/path/harbor/harbor.yml
    data_volume: /new/path

    $ vim /new/path/harbor/docker-compose.yml
    /data => /new/path

启动服务:

    $ cd /new/path
    $ docker-compose up -d
