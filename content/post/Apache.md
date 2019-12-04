---
title: "Apache"
date: 2016-09-27T03:25:26+08:00
categories: ["Web"]
tags: ["apache"]
keywords: []
author: "Canux"
draft: false
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

apache2ctl:

    // 检查配置
    $ apache2ctl configtest

a2ensite

    // 启用一个站点
    $ sudo a2ensite <site>

a2dissite

    $ sudo a2dissite <site>

a2enmod

    // 启用一个模块
    $ sudo a2enmod <mod>

a2dismod

    $ sudo a2dismod <mod>

# 配置    

配置ssl和basic auth

    # 安装创建账号的工具
    sudo apt-get install apache2-utils

    创建账号
    sudo htpasswd -c /etc/apache2/.htpasswd user
    sudo htpasswd /etc/apache2/.htpasswd another_user

启用依赖的模块:

    cd /etc/apache2/mods-enabled
    ln -sf ../mods-available/rewrite.load rewrite.load
    ln -sf ../mods-available/ssl.conf ssl.conf
    ln -sf ../mods-available/ssl.load ssl.load
    ln -sf ../mods-available/slotmem_shm.load slotmem_shm.load
    ln -sf ../mods-available/socache_shmcb.load socache_shmcb.load

修改site-avaliable/site.conf

    <VirtualHost *:80>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www

        RewriteEngine on
        RewriteCond %{SERVER_PORT} !^443$
        RewriteRule ^/(.*) https://%{HTTP_HOST}/$1 [NC,R=301,L]

        LogLevel warn
        ErrorLog /var/log/apache2/error.log
        CustomLog /var/log/apache2/access.log combined
    </VirtualHost>

    <VirtualHost *:443>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www

        // 开启ssl
        SSLEngine on
        SSLCertificateFile "/etc/apache2/user.domain.com.crt"
        SSLCertificateKeyFile "/etc/apache2/user.domain.com.key"
        BrowserMatch "MSIE [2-6]" nokeepalive ssl-unclean-shutdown downgrade-1.0 force-response-1.0
        BrowserMatch "MSIE [17-9]" ssl-unclean-shutdown

        // 开启basic auth.
        <Directory "/var/www">
            AuthType Basic
            AuthName "Restricted Content"
            AuthUserFile /etc/apache2/.htpasswd
            Require valid-user
        </Directory>

        LogLevel warn
        ErrorLog /var/log/apache2/error.log
        CustomLog /var/log/apache2/access.log combined
    </VirtualHost>