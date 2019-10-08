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

    $ openssl req  -newkey rsa:4096 -nodes -sha256 -keyout ca.key -x509 -days 365 -out ca.crt -subj "/C=CN/L=shanghai/O=lisea/CN=harbor-registry"

    $ openssl req -newkey rsa:4096 -nodes -sha256 -keyout harbor.com.key -out server.csr -subj "/C=CN/L=shanghai/O=lisea/CN=harbor.com"

    $ openssl x509 -req -days 365 -in server.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out harbor.com.crt
