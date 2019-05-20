---
title: "Kubernetes"
date: 2018-04-05T22:02:31
categories: ["Container"]
tags: ["k8s"]
keywords: []
author: "Canux"
draft: false
---

# Kubernetes

<https://github.com/kubernetes/kubernetes>

<https://github.com/kubernetes/kubeadm>

<https://github.com/kubernetes/kops>

<https://github.com/kubernetes-sigs/kubespray>

kubernetes简称k8s, 是开源的容器编排工具。

安装单机版k8s:

* minikube

安装k8s集群:

* kubeadm (k8s内置的，类似于docker swarm.)
* kops (目前主要支持aws等云平台)
* kubespray (通过ansible部署)

概念:

-: nodes, 运行pod的物理机或虚拟机.
-: namespace, 对资源和对象的抽象集合．pods/deployments/services都属于某个ns.
-: pods,一组紧密关联的容器集合，共享pid,ipc,network,uts,namespace.

k8s业务类型:

-: long-running 长期伺服型 -> RC, RS, Deployment
-: batch 批处理型-> Job
-: node-daemon 节点后台支撑型-> DaemonSet
-: stateful application 有状态应用型-> StatefulSet

api对象三大类属性:

* metadata 元数据(至少包含namespace, name, uid).
* spec 规范
* status 状态

api对象:

* ReplidcationController/rc: 控制pod运行数量
* ReplicaSet/rs: rc的升级版
* Deployments/deploy: 对集群更新
* Job
* DaemonSet/ds
* StatefulSet/sts: 控制有状态的pod, rc/rs都是控制无状态的pod.
* Services/svc: 通过service来访问pod提供的服务.为一组pod提供统一的入口,同时提供负载均衡和自动服务发现.
* Secrets
* ServiceAccount/sa

# k8s集群组成

## master

* aip-server, 提供资源操作唯一入口
* scheduler, 负责资源调度
* controller-manager, 负责维护集群状态
* etcd(可以部署单独集群), 保存整个集群的状态

## node

* kubelet, 负责维护容器生命周期, 还包括CNI CVI
* kube-proxy, 为service提供cluster内部的服务发现和负载均衡
* CRI(containerd), 创建pod

## addons

* coredns
* dashboard, web-gui
* metrics-server, 取代heapster，用于cpu/memory监控
* ingress-nginx, 为服务提供外网入口
* federation, 提供跨可用区的集群

***

# kubectl命令

<https://github.com/cloudnativelabs/kube-shell>

<https://github.com/jonmosco/kube-ps1>

<https://github.com/ahmetb/kubectx>

在master上通过kubectl命令管理集群.

    kubectl options # 查看所有命令可用选项

Basic command

    通过yaml或json文件创建pod/container:
    $ kubectl create -f FILENAME [options]

    创建serivce:
    $ kubectl expose

    创建pod/container:
    $ kubectl run (docker run)
    $ kubectl run <name> --image=<image> --labels=<key=value> ...

    $ kubectl set

    获取信息:
    $ kubectl get (docker ps)
    kubectl get nodes/no # 获取node节点信息
    kubectl get namespace/ns # 获取namespace信息
    kubectl -n kube-system get pods/po
    kubectl -n kube-system get deployments/deploy
    kubectl -n kube-system get services/svc

    $ kubectl explain

    $ kubectl edit

    $ kubectl delete

deploy command

    $ kubectl scale

    $ kubectl autoscale

    $ kubectl rollout

    $ kubectl rolling-update
    
debug&troubleshoot

    $ kubectl describe (docker inspect)

    # 获取k8s-dashboard的token
    kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep admin-user | awk '{print $1}')

    $ kubectl logs (docker logs)

    $ kubectl exec (docker exec)

    $ kubectl attach

    $ kubectl port-forward

    $ kubectl proxy

    $ kubectl cp

    $ kubectl auth

cluster management

    $ kubectl top

    $ kubectl cluster-info

    $ kubectl certificate

    $ kubectl cordon

    $ kubectl uncordon

    $ kubectl drain

    $ kubectl taint

***
