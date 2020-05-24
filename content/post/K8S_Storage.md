---
title: "Storage"
date: 2020-03-25T20:55:52+08:00
categories: ["Container"]
tags: ["k8s"]
keywords: []
author: "Canux"
draft: false
---

# Storage

kubernetes 存储.

***

# Pod Volumes

应用:
* 一个pod中的某个容器异常退出，被kubelet拉起来之前保证之前的数据部丢失.
* 同一个pod的多个容器共享数据.

pod volume类型:
1. 本地存储: emptydir/hostpath...
2. 网络存储: 

    in-tree(ks8内置支持): awsElasticBlockStore/gcePresistentDisk/nfs...

    out-of-tree(插件): flexvolume/csi...

3. projected volume: secret/configmap/downwardAPI/serviceAccountToken

***

# Persistent Volumes



***

# Persistent Volumes Claims


