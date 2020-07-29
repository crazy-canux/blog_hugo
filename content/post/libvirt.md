---
title: "Libvirt"
date: 2017-04-05T21:47:54
categories: ["Virtualization"]
tags: ["libvirt"]
keywords: []
author: "Canux"
draft: false
---

# Libvirt

支持多种虚拟化平台的库

<https://libvirt.org/>

安装:

    $ sudo apt-get install libvirt-bin (包含virsh命令和libvirtd daemon)
    // libvirtd在container中无法运行；在container中安装libvirt-bin可以获取virsh命令远程访问libvirt-bin server.

    $ sudo apt-get install libvirt-dev # 库, python/go client依赖该库

    $ sudo apt-get install virt-manager # windows管理工具

    $ sudo apt-get install virt-view # ...

    $ sudo service libvirt-bin restart

# virsh

libvirt的命令行工具

    $ virsh list --all    # 查看所有虚拟机
    $ virsh list --all --name # 只看domain name.

    $ virsh define /path/to/X.xml    # 从xml配置文件定义一个domain
    $ virsh start     # 启动虚拟机
    $ virsh reboot    # 重启虚拟机
    $ virsh shutdown   # 关闭虚拟机
    $ virsh destroy    # 强制关闭虚拟机
    $ virsh undefine   # 移除虚拟机
    $ virsh vncdisplay # 查看虚拟机的vnc信息，可以通过vnc访问.

    $ virsh net-list --all # 查看所有网络
    $ virsh net-define default.xml
    $ virsh net-autostart default
    $ virsh net-start default
    $ virsh net-undefine default
    $ virsh net-destroy default

    # 批量操作vm
    $ for vm in `virsh list --all --name`; do virsh undefine/destroy ${vm}; done

# qemu-img

创建虚拟机的镜像文件:

    $ qemu-img create -f qcow2 /path/to/name.qcow2 100G

***

# libvirt-qemu

libvirt操作qemu/kvm.

本地:

    qemu:///system
    qemu:///session
    qemu+unix:///system
    qemu+unix:///session

ssh远程:

    # libvirt-bin server上需要安装nc和libvirt-bin
    $ sudo apt-get install netcat-openbsd
    $ sudo update-alternatives --config nc // netcat-traditional避免使用默认安装

    # ssh用户需要在libvirt和kvm组里
    $ adduser username libvirtd
    $ adduser username kvm

    $ sudo apt-get install sshpass // 免密码访问

    # client上需要安装openssh-clients
    $ sshpass -p password virsh -c qemu+ssh://user@127.0.0.1/system list

tcp远程:

    $ vim /etc/libvirt/libvirtd.conf:
    listen_tls = 0　　　　　　　　　 #禁用tls登录
    listen_tcp = 1　　　　　　　　　 #启用tcp方式登录
    tcp_port = "16509"　　　　　　　 #tcp端口16509
    listen_addr = "0.0.0.0"         # 允许任意ip访问
    auth_tcp = "none"　　　　　　    #TCP不使用认证

    $ vim /etc/default/libvirt-bin
    start_libvirtd="yes"
    libvirtd_opts="-l --config /etc/libvirt/libvirtd.conf"

    $ sudo service libvirt-bin restart

    $ virsh -c qemu+tcp://127.0.0.1:16509/system list

# libvirtd配置

配置libvirtd：

    $ vim /etc/libvirt/libvirtd.conf

    max_clients = 1024　　　　　　   #最大总的连接客户数1024
    min_workers = 50　　　　　　    #libvirtd启动时，初始的工作线程数目
    max_workers = 200　　　　　　 #同上，最大数目
    max_requests = 1000　　　　　
    #最大同时支持的RPC调用，必须大于等于max_workers
    max_client_requests = 200　　 #每个客户端支持的最大连接数