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

SSL: secure sockets layer

TLS: transport layer security

CA: Certificate Authority.

SNI: server name indication

证书类型
* x509: 只有公钥没有私钥匙

编码方式
* pem: base64编码
* der: 二进制

证书文件:
* crt: 证书文件（可以是pem或der编码）
* cer: 证书文件（可以是pem或der编码）
* csr: 申请签名的文件
* key: 私钥文件

***

# 创建自签名证书

创建x509证书:

    $ openssl genrsa -out server.key 2048   // 创建key
    $ openssl req -new -key server.key -sha256 -out server.csr // 创建csr
    $ openssl x509 -req -days 365 -in server.csr -signkey server.key -sha256 -out server.crt // 创建证书

查看证书信息:

    // pem编码
    $ openssl x509 -in cert.pem -noout -text
    // der编码
    $ openssl x509 -in cert.der -inform der -noout -text

查看证书有效期:

    $ openssl x509 -in ca.domain.com.crt -noout -dates
    
# 客户端

客户端需要信任证书.

Linux:

    // 添加
    $ cp my-cert.crt /usr/local/share/ca-certificates/my-cert.crt
    $ sudo update-ca-certificates
    
    // 删除
    $ rm /usr/local/share/ca-certificates/my-cert.crt
    $ sudo update-ca-certificates --fresh
    
windows:

    // 添加
    certutil -addstore -f "ROOT" my-cert.crt
    
    // 删除
    certutil -delstore "ROOT" my-cert.crt
    
