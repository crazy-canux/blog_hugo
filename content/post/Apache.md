---
title: "Apache"
date: 2016-09-27T03:25:26+08:00
categories:
- Web
tags:
- apache
- httpd
keywords:
- apache2
draft: true
---

# Apache

<https://github.com/apache/httpd>

<http://httpd.apache.org/>

ubuntu/debian：

    sudo aptitude install apache2

redhat/centos/fedora:

    $ sudo yum install httpd2

启动服务器：

    service apache2 start

启动浏览器查看：

    http://localhost:80

***

# apache命令

a2ensite

    $ sudo a2ensite <site>

a2dissite

    $ sudo a2dissite <site>

a2enmod

    $ sudo a2enmod <mod>

a2dismod

    $ sudo a2dismod <mod>


