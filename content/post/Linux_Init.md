---
title: "Init"
date: 2019-02-03T14:04:05
categories: ["Linux"]
tags: ["init"]
keywords: []
author: "Canux"
draft: false
---

# Linux Init

linux系统启动的第一个进程,pid=1的进程.

    $ ls -l /sbin/init
    $ sudo readlink /sbin/init
    /sbin/init -> upstart
    /sbin/init -> /lib/systemd/systemd

    /etc/init.d
    The directory containing System V init scripts.
    通过service命令操作

    /etc/init
    The directory containing upstart jobs.
    通过initctl命令操作

***

# systemd

sytemd是upstart的替代版本．通过查看/sbin/init指向systemd还是upstart.

service文件位置:

    /etc/systemd/system/***.service
    /lib/systemd/system/*.service
    /usr/lib/systemd/system/*.service

service文件编写:

<https://www.freedesktop.org/software/systemd/man/systemd.unit.html#>

    [Unit]
    Description=details
    After=containerd.service # 之前启动
    Before= # 之后运行
    Bindsto= #
    Wants=containerd.service # 弱依赖
    Requires= # 强依赖
    StartLimitInterval=10s
    StartLimitBurst=5

<https://www.freedesktop.org/software/systemd/man/systemd.service.html#>

    [Service]
    Type=simple/notify/dbus/forking/idle/oneshot
    ExecStartPre=
    ExecStart=
    ExecStartPost=
    ExecStop=
    ExecStopPost=
    ExecReload=
    KillMode=node/mixed/process/control-group
    Restart=no/on-success/on-failure/on-abnormal/on-abort/on-watchdog/always # always总是开机启动，即使systemctl enable.
    RestartSec=3s # 重启之前等待的时间.
    TimeoutSec=  # TimeoutStartSec+TimeoutStopSec
    LimitNOFILE=49152 # 限制单个service的fd

    [Install]
    WantedBy=multi-user.target

systemctl命令:

    # 修改后需要重新加载.service文件
    $ systemctl daemon-reload

    $ systemctl start/stop/status ***
    $ systemctl list-unit-files
    $ systemctl show docker

    # 设置开机自动启动
    $ systemctl enable ***
    $ systemctl disable ***
    $ systemctl is-enabled ***

日志管理:

    $ sudo journalctl -fu docker.service

***

# upstart

    /etc/init.d/***
    /etc/init/***.conf

    $ sudo service docker start/stop/status/reload/restart
    $ sudo initctl start/stop/status/reload/restart docker
    $ sudo initctl list

    # 设置开机自动启动
    $ update-rc.d *** defaults
    # 取消开机启动
    $ update-rc.d *** remove

日志管理:

    $ tail -f /var/log/upstart/docker.log

Stanzas功能:

<http://upstart.ubuntu.com/wiki/Stanzas?highlight=%28%28CategoryDoc%29%29#pid>

    # 添加服务
    $ sudo vim /etc/init/docker.conf

    # docker - container daemon
    description  "docker"

    start on runlevel [2345]
    stop on runlevel [!2345]

    respawn
    # give up if restart occurs 10 times in 100 seconds.
    respawn limit 10 100

    pre-start script
        # do something before start
    end script

    script
        echo "ERROR: `date`: docker started by init."
        exec docker
    end script

    post-start script
        # do something after start
    end script

    # 检查语法错误
    $ sudo init-checkconf /etc/init/docker.conf

    # 注册服务
    $ sudo initctl reload-configuration

***
