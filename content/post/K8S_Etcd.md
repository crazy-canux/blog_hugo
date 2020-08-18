---
title: "Etcd"
date: 2020-01-10T20:55:52+08:00
categories: ["Container"]
tags: ["k8s"]
keywords: []
author: "Canux"
draft: false
---

# Etcd

<https://github.com/etcd-io/etcd>

类似的有consul和zoomkeeper.

# etcdctl

使用证书访问:

    $ etcdctl \
    --cacert=/etc/kubernetes/pki/etcd/ca.crt \
    --cert=/etc/kubernetes/pki/etcd/server.crt  \
    --key=/etc/kubernetes/pki/etcd/server.key \
    --insecure-skip-tls-verify=true \
    <command>

查看所有key

    $ etcdctl get / --prefix --keys-only
