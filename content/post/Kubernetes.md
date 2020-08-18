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

* kubeadm (k8s内置的，类似于docker swarm mode, 没有HA)
* kops (目前主要支持aws等云平台, 国内不友好)
* kubespray (通过ansible部署, 国内不友好)

k8s发行版：

* openshift-okd(redhat)
* rancher

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
* fannel
* dashboard, web-gui
* metrics-server, 取代heapster，用于cpu/memory监控
* ingress-nginx, 为服务提供外网入口
* federation, 提供跨可用区的集群

***

# 概念

k8s包含的重要概念:

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

kubernetes对外暴露服务的三种方式:

* NodePort: dev/qa. (30000-32767)
* IngressNginx: production.
* LoadBalance: cloud provider.

***

# pod

一个pod包含一个或多个容器，这些容器通过infra container共享同一个network namespace。

infra container: k8s.gcr.io/pause, 汇编写的，永远处于暂停状态。

pod共享网络:

同一个pod里面看到的网络跟infra容器看到的是一样的，一个pod只有一个ip,也就是这个Pod的network namespace对应的IP; 整个pod的生命周期跟infra容易是一样的。

pod共享存储:

通过pod volumes, 使pod中的container共享存储。

应用:
* 一个pod中的某个容器异常退出，被kubelet拉起来之前保证之前的数据部丢失.
* 同一个pod的多个容器共享数据.

pod volume类型:
1. 本地存储: emptydir/hostpath...
2. 网络存储: 

    in-tree(ks8内置支持): awsElasticBlockStore/gcePresistentDisk/nfs...

    out-of-tree(插件): flexvolume/csi...

3. projected volume: secret/configmap/downwardAPI/serviceAccountToken

pod服务质量配置:

依据容器对cpu,memory资源的request/limit需求，pod服务质量分类:

- Guaranteed
- Burstable
- BestEffort

initContainer:

initContainer用于普通Container启动前的初始化（如配置文件准备) 和 前置条件校验 (如网络)。

1. initContainer会先于普通container启动执行，直到所有initcontainer执行成功后，普通container 才会执行。
2. pod中多个initcontainer之间是按次序依次启动执行，而pod中多个普通container是并行启动。
3. initcontainer 执行成功后就结束退出，而普通container会一直执行或重启。

***

# resources

查看所有对象:

    $ kubectl api-resources

<https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.17/#statefulsetcondition-v1beta2-apps>

## Workloads

### replicasets/rs

控制无状态的pod数量,增删Pods.

### deployments/deploy

定义pod的数目和版本，通过Controller自动恢复失败的pod，滚动升级，重新生成，回滚等。

DeploymentStatus: complete, processing, failed

Deployment只负责管理不同版本的ReplicaSet, 由ReplicaSet管理Pod副本个数; 每个版本的ReplicaSet对应了Deployment template的一个版本; 一个ReplicaSet下的Pod都是相同的版本.

### daemonsets/ds

ds作用:

* 保证集群内每个节点运行一组相同的pod
* 跟踪集群节点状态，保证新加入的节点自动创建对应的pod
* 跟踪集群节点状态，保证移除的节点删除对应的pod
* 跟踪pod状态，保证每个节点pod处于运行状态

### jobs

job的作用:

* 创建一个或多个pod确保指定数量的Pod可以成功运行和终止.
* 跟踪pod状态，根据配置及时重试失败的pod.
* 确定依赖关系，保证上一个任务运行完毕后再运行下一个任务.
* 控制任务并行度，并根据配置确保pod队列大小。

### cronJobs/cj

### statefulsets/sts

控制有状态的pod数量.

***

## service discovery & load balancing

### services/svc

提供访问一个或多个pod的稳定的访问地址.支持ClusterIP, NodePort, LoadBalancer等访问方式.

### ingresses

### endpointslices

***

## Config & Storage

### secrets

secret用于在集群中存储密码，token等敏感信息用的资源对象.

其中敏感数据采用base-64编码保存.

四种类型:

* Opaque
* kubernetes.io/service-account-token
* kubernetes.io/dockerconfigjson
* bootstrap.kubernetes.io/token

secret主要被pod使用，一般通过volume挂载到指定容器目录，供容器中业务使用.

访问私有镜像仓库也可以通过secret实现.

secret大小限制1M; secret不适合机密信息，推荐用vault.

### configmaps/cm

主要管理容器运行所需的配置文件，环境变量，命令行参数等可变配置。

可用于解耦容器镜像和可变配置，从而保障工作负载的可移植性.

主要被pod使用，一般用于挂载pod用的配置文件，环境变量，命令行参数.

ConigMap 大小不超过1M; pod只能引用相同namespace中的configmap, pod引用的configmap不存在时，pod无法创建;使用envFrom从ConfigMap配置环境变量时，如果ConfigMap中的某些key被认为无效，该环境变量不会注入容器，但是pod可以创建; 只有通过k8s api创建的pod才能使用ConfigMap, 其它方式创建的pod不能使用ConfigMap.

### persistentvolume/pv

static volume provisioning: 

dynamic volume provisioning:

### persistentvolumeclaims/pvc

pvc中只需要申明需要的存储size, access mode等业务需求.

pvc简化了用户对存储的需求，pv才是存储的实际信息载体，通过kube-controller-manager中的PersistentVolumeController将PVC与合适的PV 绑定.

### csidrivers

### csinodes

### storageclasses/sc

### volumeattachments

***

## metadata

### events

### horizontalpodautoscaler

### podtemplate

***

## cluster

### componentstatuses/cs

### namespaces/ns

一个集群内部的逻辑隔离机制，每个资源都属于一个namespace，同一个namespace中的资源命名唯一，不同namespace中的资源可重名.

### nodes/no


### serviceaccount/sa

解决pod在集群中的身份认证问题.

***