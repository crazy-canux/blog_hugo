---
title: "Redis"
date: 2017-05-03T14:46:14
categories: ["Database"]
tags: ["redis"]
keywords: []
author: "Canux"
draft: false
---

# Redis

<http://redisdoc.com/>

redis在key-value存储上性能比memcached更好．

安装：

    # redis-cli, redis-sentinel, redis-server
    $ sudo apt-get install redis-server

只安装redis-cli:

    $ sudo apt-get install redis-tools

redis-server监听端口6379.

redis-sentinel端口26379.

redis gui:

<https://github.com/uglide/RedisDesktopManager>

***

# redis的命令

server:

    redis-server

client:

    redis-client

test:

    redis-benchmark

sentinel:

    redis-sentinel

***

# CLI

redis-cli 进入命令行模式

    > command    # 查看所有可用命令

    > info    # 查看redis服务器信息

    > monitor

***

# 数据类型

string

    > set <key> <value>
    > get <key>

list

    > lset <key> <index> <value>
    > lindex <key> <index>
    > rpop
    > lpop <key>
    > rpush
    > lpush

hash

    > hset <key> <field> <value>
    > hget <key> <field>

set

    > sadd <key> <member>
    > spop <key>
    > srem <key> <memeber>

sorted set

    > zadd <key> <score> <member>
    > zrem <key> <member>

***

# Monitoring

通过redis-cli> info查看

    其它指标参考grafana dashboard.

    Redis_mode: cluster/standalone

    >>> replication (master/slave)
    Connected_slaves:    连接的slave实例个数

    >>> persistence  （rdb和aof的持久化信息）
