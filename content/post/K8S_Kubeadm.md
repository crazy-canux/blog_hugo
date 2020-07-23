---
title: "Kubeadm"
date: 2019-12-30T21:47:17+08:00
categories: ["Container"]
tags: ["k8s"]
keywords: []
author: "Canux"
draft: false
---

# kubeadm

<https://kubernetes.io/zh/docs/setup/independent/create-cluster-kubeadm/>

kubeadm是k8s自带的部署集群的工具.

# Install

在每台机器上安装 kubeadm, kubelet, kubectl:

    $ curl -fsSL https://mirrors.aliyun.com/kubernetes/apt/doc/apt-key.gpg | sudo apt-key add - 
    $ echo "deb https://mirrors.aliyun.com/kubernetes/apt/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
    $ sudo apt-get update
    $ sudo apt-get --yes --allow-unauthenticated install kubeadm kubelet kubectl
    $ sudo apt-mark hold kubelet kubeadm kubect

# Kubeadm

init:

    $ kubeadm init 
    --config <config>
    --apiserver-advertise-address <master> // 多网卡指定网卡IP
    --image-repository <registry> // default k8s.gcr.io
    --kubernetes-version <version>
    --pod-network-cidr <cidr> // 指定pod的cidr

join:

    $ kubeadm join [apiserver-advertise-address] --token <token> --discovery-token-ca-cert-hash <hash>

reset:

    $ kubeadm reset -f/--force

token:
  
    $ kubeadm token create/delete/generate/list

***

# 部署Cluster

## 部署master

关闭swap

    $ sudo swapoff -a

初始化

    $ sudo kubeadm init \
    // --config=/var/lib/kubelet/config.yaml \
    --pod-network-cidr=10.244.0.0/16 \
    --apiserver-advertise-address=<IP> \
    --kubernetes-version=v1.17.0 \
    --image-repository=registry.aliyuncs.com/google_containers \
    -v=6
    // --config 一般使用默认即可.
    // --pod-network-cidr=10.244.0.0/16 是固定用法，表示选择flannel为网络插件。
    // --image-repository 指定registry, 默认是gcr

配置当前帐号

    mkdir -p $HOME/.kube
    sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    sudo chown $(id -u):$(id -g) $HOME/.kube/config

部署网络插件flannel

    // 在所有node上部署cni-plugin:
    // <https://github.com/containernetworking/plugins/releases>
    $ sudo mkdir -p /opt/cni/bin
    // 下载并解压所有插件命令到该目录.

    $ kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

配置master是否部署pod

    # enable master deploy pod (默认不部署pod到master)
    kubectl taint nodes --all node-role.kubernetes.io/master-

    # disable master deploy pod
    kubectl taint nodes <node> node-role.kubernetes.io/master=true:NoSchedule

## 部署node

    $ sudo swapoff -a

    // 在所有node上部署cni-plugin:
    // <https://github.com/containernetworking/plugins/releases>
    $ sudo mkdir -p /opt/cni/bin
    // 下载并解压所有插件命令到该目录.

    $ sudo kubeadm join 192.168.1.1:6443 \
    --token 8po0v5.m1qlbc7w0btq15of \
    --discovery-token-ca-cert-hash sha256:21d8365e336d5218637ddf26e2ec5d91c7dd2de518dbe47973e089837b13265b

## 验证

    $ kubectl get pods -n kube-system
    $ kubectl get nodes

## 删除cluster

所有node运行:

    $ sudo kubeadm reset -f

所有node上删除flannel的网络配置

    $ sudo ifconfig cni0 down
    $ sudo brctl delbr cni0
    $ sudo ifconfig flannel.1 down

所有node清空iptables

    $ sudo iptables -F
    $ sudo iptables -X
    $ sudo iptables -t nat -F
    $ sudo iptables -t nat -X

***

# metrics-server

<https://github.com/kubernetes-sigs/metrics-server>

    # deploy 0.3.6
    # 修改image为  registry.aliyuncs.com/google_containers/metrics-server-amd64:v0.3.6
    $ kubectl apply -f ./components.yaml

***

# dashboard

<https://github.com/kubernetes/dashboard>

    // https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta8/aio/deploy/recommended.yaml 修改服务类型为NodePort
    kind: Service
    apiVersion: v1
    metadata:
      labels:
        k8s-app: kubernetes-dashboard
      name: kubernetes-dashboard
      namespace: kubernetes-dashboard
    spec:
      # use nodeport
      type: NodePort
      ports:
        - port: 443
        targetPort: 8443
        # specify nodeport
        nodePort: 30001
    selector:
      k8s-app: kubernetes-dashboard
    // 部署dashboard
    $ kubectl apply -f dashboard.yaml

    // ServiceAccount 和 ClusterRoleBinding
    apiVersion: v1
    kind: ServiceAccount
    metadata:
      name: admin-user
      namespace: kubernetes-dashboard
    ---
    apiVersion: rbac.authorization.k8s.io/v1
    kind: ClusterRoleBinding
    metadata:
      name: admin-user
    roleRef:
      apiGroup: rbac.authorization.k8s.io
      kind: ClusterRole
      name: cluster-admin
    subjects:
      - kind: ServiceAccount
        name: admin-user
        namespace: kubernetes-dashboard
    // 创建admin账号
    $ kubectl apply -f auth.yaml

    // check
    $ kubectl -n kubernetes-dashboard get pods --watch

    // 获取token
    $ kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '{print $1}')

    // 使用admin帐号的token登录
    > https://<IP>:30001

    // 删除已安装的dashboard
    $ kubectl delete ns kubernetes-dashboard
