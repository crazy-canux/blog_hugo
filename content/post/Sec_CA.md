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

    $ openssl req -newkey rsa:4096 -nodes -sha256 -keyout ca.key -x509 -days 365 -out ca.crt -subj "/C=CN/L=shanghai/O=lisea/CN=harbor-registry"

    $ openssl req -newkey rsa:4096 -nodes -sha256 -keyout <domain>.key -out <domain>.csr -subj "/C=CN/L=shanghai/O=lisea/CN=<domain>"

    $ openssl x509 -req -days 365 -in <domain>.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out <domain>.crt