---
title: "Helm"
date: 2019-09-05T22:02:31
categories: ["Container"]
tags: ["helm"]
keywords: []
author: "Canux"
draft: false
---

# Helm

helm有两个组件:

* helm: 客户端
* tiller: 服务端(helm3被移除)

概念:

* chart: 一个helm包，包含运行一个应用所需的镜像，依赖和资源.
* release: 在k8s集群上运行的一个chart实例.
* repository: 用于发布和存储chart的仓库.

# 安装

helm和kubectl一样，访问指定配置的k8s集群。

本地二进制安装helm:

    // 下载helm二进制文件
    $ mv linux-amd64/helm /usr/local/bin/helm && chmod +x /usr/local/bin/helm

helm2需要安装tiller到k8s上:

    $ docker pull registry.aliyuncs.com/google_containers/tiller:v2.16.1
    $ kubectl --kubeconfig=kube_configxxx.yml -n kube-system create serviceaccount tiller
    $ kubectl --kubeconfig=kube_configxxx.yml create clusterrolebinding tiller \
    --clusterrole cluster-admin --serviceaccount=kube-system:tiller

    $ helm init --kubeconfig=$kubeconfig --service-account tiller --skip-refresh 

helm3+不再需要安装tiller,也不需要helm init.

# 命令

安装chart:

    helm install [name] [chart] [flags]
    helm install redis ./redis

查看release:

    helm list

删除release:

    helm delete 

***

# chart

    Chart.yaml
    values.yaml
    requirements.yaml
    templates/deployment.yaml
    charts/