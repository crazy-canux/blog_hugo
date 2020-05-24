---
title: "Exporter"
date: 2018-01-18T19:23:25
categories: ["DevOps"]
tags: ["monitoring"]
keywords: []
author: "Canux"
draft: false
---

# exporter

广义上讲所有可以向Prometheus提供监控样本数据的程序都可以被称为一个Exporter。而Exporter的一个实例称为target，如下所示，Prometheus通过轮询的方式定期从这些target中获取样本数据:

prometheus通过pull从exporter拉取数据.

直接采集:cAdvisor, kubernetes, etcd, gokit等直接内置了用于向prometheus暴露监控数据的端点.

间接采集: 通过promethesu的client api编写exporter，例如mysql-exporter, consul-exporter.

***

# host

<https://github.com/prometheus/node_exporter>

***

# k8s

k8s metrics api包括 resource metrics api 和 customer metrics api.

cadvisor, metrics-server, kube-state-metrics 等实现了 核心api.

一些adapter实现了自定义api.

k8s-prometheus-adapter实现了metrics-server的核心api，同时实现了自定义api.

## metrics-server 

提供了整个集群的资源监控数据

<https://github.com/kubernetes-sigs/metrics-server>

## cAdvisor 

负责单节点内部的容器和节点资源使用统计，会自动收集本机容器 CPU、内存、网络和文件系统的资源占用情况

<https://github.com/google/cadvisor>

## kube-state-metrics 

提供了 Kubernetes 资源对象（如 DaemonSet、Deployments 等）的度量。

<https://github.com/kubernetes/kube-state-metrics>

## adapter

同时支持核心api和自定义api.

<https://github.com/DirectXMan12/k8s-prometheus-adapter>

***

# blackbox

<https://github.com/prometheus/blackbox_exporter>

# wmi

<https://github.com/martinlindhe/wmi_exporter>

# snmp

<https://github.com/prometheus/snmp_exporter>

# mysql

<https://github.com/prometheus/mysqld_exporter>

# rabbitmq

<https://github.com/deadtrickster/prometheus_rabbitmq_exporter>

# mongo

<https://github.com/percona/mongodb_exporter>

# redis

<https://github.com/oliver006/redis_exporter>

# haproxy

<https://github.com/prometheus/haproxy_exporter>

# consul

<https://github.com/prometheus/consul_exporter>

***

# pushgateway

<https://github.com/prometheus/pushgateway>

由于Prometheus数据采集基于Pull模型进行设计，因此在网络环境的配置上必须要让Prometheus Server能够直接与Exporter进行通信。 当这种网络需求无法直接满足时，就可以利用PushGateway来进行中转。可以通过PushGateway将内部网络的监控数据主动Push到Gateway当中。而Prometheus Server则可以采用同样Pull的方式从PushGateway中获取到监控数据.