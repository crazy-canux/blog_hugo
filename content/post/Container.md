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

OCI: Open Container Initiative: runc

CRI: Container Runtime Interface: docker-containerd, rkt

CNI: Container Network Interface.

CSI: Container Storage Interface.

# nsenter

nsenter - run program with namespaces of other processes

    $ nsenter [options] [program [arguments]]

    $ nsenter -a --all

    $ nsenter -n/--net <namespace> <command>
    $ nsenter --net=/var/run/docker/netns/lb_atz8r3bly ifconfig

    $ nsenter -m/--mount <namespace> <command>