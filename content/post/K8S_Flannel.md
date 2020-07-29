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

删除插件:

删除插件会影响已经部署的pod.

    // 删除flannel 
    $ kubectl delete -f X.yml  
    $ sudo systemctl stop kubelet docker

    // 第二步，在node节点清理flannel网络留下的文件
    ifconfig cni0 down
    ip link delete cni0 
    ifconfig flannel.1 down
    ip link delete flannel.1 
    rm -rf /var/lib/cni /etc/cni /run/flannel
    $ sudo rm -rf /var/lib/kubelet /var/lib/etcd
    
    // 重启kubelet
    $ sudo systemctl start kubelet docker

# kubeadm

kubeadm init必须指定flannel的Network参数:

    --pod-network-cidr=10.244.0.0/16

# configuration

/etc/kube-flannel/net-conf.json

    {
      "Network": "10.244.0.0/16",
      "SubnetLen": 24,
      "SubnetMin": "10.244.0.0",
      "SubnetMax": "10.244.255.0",
      "Backend": {
        "Type": "vxlan"
      }
    }