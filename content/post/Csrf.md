---
title: "Csrf"
date: 2018-07-28T16:08:54
categories:
- Security
tags:
- csrf
keywords:
- csrf
draft: true
---

# CSRF

Cross-site request forgery, 跨站请求伪造．

发生条件：

* 登陆受信任网站A, 并在本地生成cookie
* 在不退出A的情况下，访问危险网站B

预防方法:

* 正确使用get,post和cookie
* 在非get请求中增加伪随机数


