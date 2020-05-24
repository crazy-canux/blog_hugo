---
title: "Prometheus"
date: 2018-01-18T19:23:25
categories: ["DevOps"]
tags: ["monitoring"]
keywords: []
author: "Canux"
draft: false
---

# Prometheus

Prometheus Server是Prometheus组件中的核心部分，负责实现对监控数据的获取，存储以及查询

swarm部署:

<https://github.com/vegasbrianc/prometheus>

k8s部署:

<https://github.com/coreos/kube-prometheus>

<https://github.com/coreos/prometheus-operator>

<https://github.com/helm/charts/tree/master/stable/prometheus-operator>

高可用:

<https://github.com/thanos-io/thanos>

prometheus grafana dashboard:

<https://github.com/kubernetes-monitoring/kubernetes-mixin>

<https://github.com/grafana/kubernetes-app>

***

# PromQL

metrics类型:
1. counter计数器
2. gauge仪表盘
3. histogram直方图
4. summary摘要

