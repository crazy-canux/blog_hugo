---
title: "Dashboard"
date: 2020-01-04T20:03:39+08:00
categories: ["Container"]
tags: ["k8s"]
keywords: []
author: "Canux"
draft: false
---

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
