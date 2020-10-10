---
title: "CA"
date: 2018-07-28T16:08:54
categories: ["Security"]
tags: ["CA"]
keywords: []
author: "Canux"
draft: false
---

# CA

Certificate Authority.

***

# 创建自签名证书

创建x509证书:

    $ openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout ca.key -out ca.crt -subj "/CN=k8s.devops.com/O=k8s.devops.com"

查看证书有效期:

    $ openssl x509 -in /etc/ssl/ca.domain.com.crt -noout -dates