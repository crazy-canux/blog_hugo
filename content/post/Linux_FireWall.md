---
title: "Firewall"
date: 2016-03-31T21:48:59
categories: ["Linux"]
tags: ["firewall"]
keywords: []
author: "Canux"
draft: false
---

# Firewall

UFW: linux防火墙配置工具，底层还是调用iptables.

filewall: centos的防火墙命令, 底层还是调用iptables.

***

# SELinux

Security-Enhanced-Linux

本地安全

***

# Netfilter

网络安全

***

# iptables

通过iptables操作Netfilter实现应用层安全.

table:

    filter 默认表
    nat
    mangle
    raw
    security

## filter

chain:

    INPUT
    FORWARD
    OUTPUT

## nat

chain:

    INPUT
    OUTPUT
    PREROUTING
    POSTROUTING

postrouting:

    snat: 内网主机访问外网经过路由时，源ip会发生变化。

prerouting:

    dnat:  外网访问内网经过路由时，目的ip会发生变化。

## iptables命令

    -L/--list  [chain [ rulenum]]
    -S/--list-rules [chain [rulenum]]
    -Z/--zero [chain [rulenum]]

    -A/--append chain
    -C/--check chain
    -N/--new chain

    -F/--flush [chain] // 删除chain中的rules.
    -X/--delete-chain [chain] // 删除自定义chain.

    -R/--replace chain rulenum
    -D/--delete chain [rulenum]
    -I/--insert chain [rulenum]

    -P/--policy chain target

    -E/--rename-chain old-chain new-chain

tables:

    -t/--table   filter/nat/mangle

options:

    [!] -p/--protocol
    [!] -s/--source
    [!] -d/--destination
    [!] -o/--out-interface
    [!] -i/--in-interface
    [!] -f/--fragment
    --dport    destination-port
    --sport    source-port
    -m, --match
    -j, --jump
    -g, --goto
    -c, --set-counters

    --line-number  # 显示rulenum
    -v/--verbose
    -n/--numeric

others:

    # 保存规则
    iptables-save > firewall.txt
    # 加载规则
    iptables-restore firewall.txt

开机启动:

    iptables-save > /etc/iptables.rules
    vim /etc/network/if-pre-up.d/iptables
    #!/bin/bash
    iptables -F
    iptables -X
    iptables -t nat -F
    iptables -t nat -X
    iptables-restore /etc/iptables.rules

