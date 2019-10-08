---
title: "Ssh"
date: 2016-07-28T15:53:34
categories: ["Network"]
tags: ["ssh"]
keywords: []
author: "Canux"
draft: false
---

# OpenSSH

<http://www.openssh.com/>

windows上支持ssh协议的客户端：

* putty
* xshell
* MobaXterm
* secureCRT

安装：

    $ sudo apt-get install openssh-server
    $ sudo apt-get install openssh-client

***

# SSH命令

ssh是openssh协议的客户端．

远程操作的命令包括ssh, scp, sftp.

ssh

    $ ssh
    # 远程执行命令需要用双引号，不能用单引号
    $ ssh username@host "command/script"

scp

    $ scp

sftp

    $ sftp

常用选项：

    -C   compression
    # 不需要输入yes来交互, 或者修改/etc/ssh/ssh_config
    -o StrictHostKeyChecking=no
    -o UserKnownHostsFile /dev/null

ssh也包括一些密钥管理的命令.

ssh-keygen

    $ ssh-keygen -t rsa -C 'canuxcheng@gmail.com'

    # 通过将本机的公钥拷贝到远程机器实现无密码访问．
    # 将本机的public-key拷贝到远程机器的authorized_keys.
    $ ssh-copy-id -i ~/.ssh/id_rsa.pub user@remote
    # 另外的拷贝方法
    $ ssh user@host "cat >> ~/.ssh/authorized_keys" < ~/.ssh/id_rsa.pub
    $ sudo service ssh restart # 需要重启ssh服务

    非交互式通过命令行传密码的命令：
    $ sshpass -p [password]

ssh-add

ssh-keysign

ssh-keyscan

***

# 启用root远程ssh

    $ sudo -i
    # passwd root
    # vim /etc/ssh/sshd_config
    > PermitRootLogin prohibit-password
    > PermitRootLogin yes
    # service ssh restart

# ssh tunel

SSH Tunnel有三种

* 本地Local（ssh -NfL）, 在ssh client(本地)执行.

        正向代理：（a能ssh到b，b能访问c，实现a访问c，相当于a是ssh client）A 通过 中转B连接 server C.

        在A 上建立tunnel:
        ssh -Nf -L  ssh-client-ip:ssh-client-port:remoteServerIp:remoteServerPort  user@agentIp

       访问ssh-client-ip/A的ssh-client-port 即可访问 remoteServerIp/C 的 remoteServerPort 提供的服务.

* 远程Remote（ssh -NfR）, 在ssh server执行.

        反向代理：（b能ssh到a，b能访问c，实现a访问c, a相当于ssh server, c相当于remote server) A 通过中转B访问server C.

        在B上建立tunnel:
        ssh -Nf -R  ssh-server-ip:ssh-server-port:remoteServerIp:remoteServerPort    user@agent

        访问ssh-server-ip/A的ssh-server-port端口即可访问remoteServerIp/C 的 remoteServerPort 提供的服务。

* 动态Dynamic（ssh -NfD）

        socks代理： a能ssh到B，b能访问一个网段，实现a访问该网段.

        在a上建立tunnel
        ssh -D [ssh-client-ip]:ssh-client-port user@agent

        socks代理配置ssh-client-ip:ssh-client-port 即可通过B访问该网段。
