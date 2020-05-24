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

    $ echo "deb https://mirrors.aliyun.com/kubernetes/apt/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
    $ sudo apt-get update
    $ sudo apt-get --yes --allow-unauthenticated install kubeadm kubelet kubectl
    $ apt-mark hold kubelet kubeadm kubect

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

初始化master:

    $ sudo swapoff -a

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

部署node:

    $ sudo swapoff -a

    $ sudo kubeadm join 192.168.1.1:6443 \
    --token 8po0v5.m1qlbc7w0btq15of \
    --discovery-token-ca-cert-hash sha256:21d8365e336d5218637ddf26e2ec5d91c7dd2de518dbe47973e089837b13265b

允许将pod部署在master节点(默认不会):

    $ kubectl taint nodes --all node-role.kubernetes.io/master-

# 删除cluster

所有node运行:

    $ sudo kubectl reset -f

所有node上删除flannel的网络配置

    $ sudo ifconfig cni0 down
    $ sudo brctl delbr cni0
    $ sudo ifconfig flannel.1 down

***

# flannel

此时状态是notReady, 需要网络插件

    $ kubectl get nodes (notReady)

在所有node上部署cni-plugin:

<https://github.com/containernetworking/plugins/releases>

    $ sudo mkdir -p /opt/cni/bin
    // 下载并解压所有插件命令到该目录.

network-addon(master上操作即可):

    $ kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

veryfy:

    $ kubectl get nodes
    $ kubectl get pod --all-namespaces

***

# ingress-nginx

<https://github.com/kubernetes/ingress-nginx>

    // 部署
    $ kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/mandatory.yaml
    $ kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/baremetal/service-nodeport.yaml

    // 验证部署
    $ kubectl get pods --all-namespaces -l app.kubernetes.io/name=ingress-nginx --watch

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

    // 获取token
    $ kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '{print $1}')

    // 使用admin帐号的token登录
    > https://<IP>:30001

    // 删除已安装的dashboard
    $ kubectl delete ns kubernetes-dashboard

# metrics-server

<https://github.com/kubernetes-sigs/metrics-server>

    # deploy 0.3.6
    # 修改image为  registry.aliyuncs.com/google_containers/metrics-server-amd64:v0.3.6
    $ kubectl apply -f ./components.yaml


