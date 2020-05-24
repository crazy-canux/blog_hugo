---
title: "Flannel"
date: 2019-12-30T22:06:48+08:00
categories: ["Container"]
tags: ["network"]
keywords: []
author: "Canux"
draft: false
---

# Flannel

flannel是k8s最常用的网络插件.

<https://github.com/coreos/flannel>

在所有node上部署cni-plugin:

<https://github.com/containernetworking/plugins/releases>

    $ sudo mkdir -p /opt/cni/bin
    // 下载并解压所有插件命令到该目录.

network-addon(master上操作即可):

    $ kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

veryfy:

    $ kubectl get nodes
    $ kubectl get pod --all-namespaces

重新部署网络插件:

    $ kubectl delete -f X.yml  

    // 第二步，在node节点清理flannel网络留下的文件
    ifconfig cni0 down
    ip link delete cni0
    ifconfig flannel.1 down
    ip link delete flannel.1
    rm -rf /var/lib/cni/
    rm -f /etc/cni/net.d/*
    
    // 重启kubelet
    $ sudo systemctl restart kubelet