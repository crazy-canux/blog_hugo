---
title: "HAProxy"
date: 2018-07-28T16:08:54
categories: ["Network"]
tags: ["haproxy"]
keywords: []
author: "Canux"
draft: false
---

# HAProxy

<https://www.haproxy.com/>

安装:

    $ sudo apt-get install haproxy

配置:

    $ sudo vim /etc/haproxy/haproxy.cfg

global:

    mode: http | tcp | health

defaults:

frontend:

backend:

listen:
