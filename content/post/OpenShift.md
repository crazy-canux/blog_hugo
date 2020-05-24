---
title: "OpenShift"
date: 2019-12-02T21:43:24+08:00
categories: ["Container"]
tags: ["k8s"]
keywords: []
author: "Canux"
draft: false
---

# OpenShift

readhat kubernetes.

# Deploy

pre-install:

    > check "sysctl net.ipv4.ip_forward" is set to 1

    /etc/containers/registries.conf
    [registries.insecure]
    registries = ['172.30.0.0/16']

    /etc/docker/daemon.json
    {
        "insecure-registries": [
            "172.30.0.0/16"
        ]
    }

    $ sudo systemctl daemon-reload
    $ sudo systemctl restart docker

    docker network inspect -f "{{range .IPAM.Config }}{{ .Subnet }}{{end}}" bridge

    # firewall-cmd --permanent --new-zone dockerc
    # firewall-cmd --permanent --zone dockerc --add-source 172.17.0.0/16
    # firewall-cmd --permanent --zone dockerc --add-port 8443/tcp
    # firewall-cmd --permanent --zone dockerc --add-port 53/udp
    # firewall-cmd --permanent --zone dockerc --add-port 8053/udp
    # firewall-cmd --reload

install:

    > download binary and put in $PATH
    # oc cluster up --public-hostname='your.domain.com' --base-dir=/opt/oc

    # firewall-cmd --zone=public --add-port=8443/tcp --permanent
    # firewall-cmd --reload

test:

    > https://your.domain.com:8443

# oc

oc command

    $ oc cluster up --enable [*]
    $ oc cluster add --base-dir=/opt/oc service-catalog
