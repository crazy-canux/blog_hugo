---
title: "Dashboard"
date: 2020-01-04T20:03:39+08:00
categories: ["Container"]
tags: ["k8s"]
keywords: []
author: "Canux"
draft: false
---

# dashboard

* kubernetes-dashboard
* lens
* octant

***

# kubernetes-dashboard

<https://github.com/kubernetes/dashboard>

    // 部署dashboard
    $ kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.3/aio/deploy/recommended.yaml

    // check
    $ kubectl -n kubernetes-dashboard get pods --watch

    ---
    apiVersion: v1
    kind: ServiceAccount
    metadata:
      name: dashboard-admin
      namespace: kubernetes-dashboard
    ---
    apiVersion: rbac.authorization.k8s.io/v1
    kind: ClusterRoleBinding
    metadata:
      name: kubernetes-dashboard-admin
      namespace: kubernetes-dashboard
    roleRef:
      apiGroup: rbac.authorization.k8s.io
      kind: ClusterRole
      name: cluster-admin
    subjects:
      - kind: ServiceAccount
        name: dashboard-admin
        namespace: kubernetes-dashboard
    // 创建admin账号
    $ kubectl apply -f auth.yaml

    // 获取token
    $ kubectl -n kubernetes-dashboard describe secret \
    $(kubectl -n kubernetes-dashboard get secret | \ 
    grep dashboard-admin | awk '{print $1}')

    // 使用admin帐号的token登录
    > https://<IP>:30001

    // 删除已安装的dashboard
    $ kubectl delete ns kubernetes-dashboard

## authentication

使用basic auth:

    --authentication-mode=basic

## access dashboard

本机访问

    $ kubectl proxy
    #> http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/

远程访问

    $ kubectl port-forward -n kubernetes-dashboard service/kubernetes-dashboard 8080:443

nodePort:

    kind: Service
    apiVersion: v1
    metadata:
      labels:
        k8s-app: kubernetes-dashboard
      name: kubernetes-dashboard
      namespace: kubernetes-dashboard
    spec:
      type: NodePort
      ports:
        - port: 443
        targetPort: 8443
        nodePort: 30001
    selector:
      k8s-app: kubernetes-dashboard

    #> https://<node-ip>:30001

ingress:

***

# metrics-server

<https://github.com/kubernetes-sigs/metrics-server>

    # deploy 0.3.6
    # 修改image为  registry.aliyuncs.com/google_containers/metrics-server-amd64:v0.3.6
    $ kubectl apply -f ./components.yam
