---
title: "kernel"
date: 2020-05-27T22:29:00+08:00
categories: ["Linux"]
tags: ["kernel"]
keywords: []
author: "Canux"
draft: false
---

# Kernel

# command

    lsmod 查看已加载的模块    # /proc/modules
    rmmod <name> 删除模块

    modprobe -c 查看已编译可加载的内核模块
    modprobe <name> 加载模块 # /etc/modules
    modprobe -r <name> 删除模块

    // modprobe 重启就没了
    echo "ipmi_devintf" >> /etc/modules