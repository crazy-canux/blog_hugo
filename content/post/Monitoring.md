---
title: "Monitoring"
date: 2016-06-08T09:46:47
categories: ["DevOps"]
tags: ["monitoring"]
keywords: []
author: "Canux"
draft: false
---

# Monitoring

Tranditional monitoring is for Datacenter, like nagios, zabbix.

Modern monitoring is for Cloud and Container.

Modern monitoring based on metrics, logs and tracing.

现代的监控系统在DevOps的基础上，除了传统的metrics和logging的采集，还需要tracing应用。

***

# metrics

## TICK stack

influxdb: tsdb.

kapacitor: alerting.

chronograf: GUI.

telegraf(agent): metrics collector.

## Prometheus

## Graphing

最流行的监控绘图软件是grafana, 支持influxdb,elasticsearch和prometheus.

***

# Logging

## ELK

## Loki+Grafana

***

# Tracing

## Jeager

