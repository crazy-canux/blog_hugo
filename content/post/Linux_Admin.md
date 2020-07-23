---
title: "Admin"
date: 2016-04-03T14:04:05
date: 2016-04-15T09:41:39+08:00
categories: ["Linux"]
tags: ["admin"]
keywords: []
author: "Canux"
draft: false
---

# Linux Admin

dpkg: ubuntu, debian.

rpm: fedora, centos, redhat.

zypper: suse.

***

# Linux系统常用的安装和配置

## virtualbox

开机自动挂载共享文件夹

    # 手动挂在命令, 需要安装增强功能
    $ mount -t vboxsf FolderNameOnWindows /path/on/linux

    # 实现开机自动挂载
    $ sudo vim /etc/rc.local
    mount.vboxsf -w ShareFolderNameOnWindows MountPointOnLinux

## xrdp

从windows的RDP远程连接linux.

use RDP on windows to connect to ubuntu16.04.

    sudo dpkg -i tigervncserver_1.6...deb # download and install tigervncserver first.
    sudo apt-get install -f
    sudo apt-get instal xrdp -y
    echo unity > ~/.xsession

use RDP on windows to connect to ubuntu14.04.

    sudo apt-get install xrdp
    sudo apt-get install xfce4
    echo xfce4-session > ~/.xsession
    sudo vim /etc/xrdp/startwm.sh
    # add 'startxfce4' to last line.
    sudo service xrdp restart

## 清理内存的buff/cache

    echo 3 > /proc/sys/vm/drop_caches
    # reboot才能改回默认的0

## 终端现实超大艺术字

    $ sudo apt-get install figlet
    $ figlet <text>

***

# Ubuntu/Debian安装后的基本配置

    sudo apt-get install build-essential make libssl-dev

## apt mirror

    vim /etc/apt/sources.list
    # deb http://ip:port/path   ubuntu16/
    # deb [trusted=yes] https://<user>:<pw>@ip:port/path   ubuntu16/
    deb [trusted=yes] https://user:pw@mirror.com/mirror ubuntu16/

    vim /etc/apt/apt.conf
    Acquire::https::mirror.com::Verify-Peer "false";
    Acqhire::https::mirror.com::Verify-Host "false";

## 中文输入法

安装一个中文输入法框架fcitx(IBus, SCIM, UIM)：

    $ sudo apt-get install fcitx

安装一种输入法引擎：

    sudo apt-get install fcitx-googlepinyin
    sudo apt-get install fcitx-sunpinyin
    sudo apt-get install fcitx-libpinyin
    sudo apt-get install fcitx-sougoupinyin
    sudo apt-get install fcitx-cloudpinyin

配置程序：

    kcm-fcitx - for qt - <https://github.com/fcitx/kcm-fcitx>
    fcitx-configtool - for gtk - <https://github.com/fcitx/fcitx-configtool>

在键盘输入方式系统从ibus改为fcitx，然后重启。

## dconf修改配置

也可以通过系统自带的dconf命令修改．

    $ sudo apt-get install dconf-editor

gedit打开txt文件乱码

    # org->gnome->gedit->preferences->encodings->auto-detected 添加'GB2312','GBK',...

开启远程桌面无密码登陆

    $ dconf write /org/gnome/desktop/remote-access/require-encryption false
    or
    # org->gnome->desktop->remote-access->require-encryption false

## 挂载U盘失败

移动硬盘或者u盘不能挂载，删掉/etc/fstab的关于sdb的行，保存后重新插拔。

## 创建桌面图标（比如eclipse）

    cd /usr/share/applications
    sudo vi XXX.desktop

添加必要属性后拖到桌面或启动栏即可。

## 安装QQ

    sudo apt-get install libgtk2.0-0:i386
    sudo apt-get install lib32ncurses5
    sudo apt-get install -f
    sudo dpkg -i fonts-wqy-microhei_0.2.0-beta-2_all.deb
    sudo dpkg -i ttf-wqy-microhei_0.2.0-beta-2_all.deb
    sudo dpkg -i wine-qqintl_0.1.3-2_i386.deb

## 安装文档的包

手册位于/usr/share/man

    sudo apt-get install glibc-doc manpages-dev manpages-posix-dev manpages-zh

## 记录终端操作

安装相关工具:

    $sudo apt-get install ttyrec
    $sudo apt-get install imagemagick
    $hg clone https://bitbucket.org/antocuni/tty2gif

开始记录:

    $ttyrec

在终端播放记录文件ttyrecord:

    $ttyplay ttyrecord

将ttyrecord文件转化成gif文件:

    $tty2gif.py typing ttyrecord

将多个gif文件合并成一个文件:

    $convert -limit memory 2mb -limit map 2mb -delay 2 -loop 0 *.gif example.gif

## 添加用户为管理员

    $ vim /etc/sudoers
    user     ALL = (ALL:ALL) ALL

## Ubuntu网络设置

ubuntu修改hostname:

    $ sudo vim /etc/hostname
    new-hostname
    $ sudo vim /etc/hosts
    ip-address hostname
    $ sudo reboot

设置系统代理:

    $ vim /etc/profile.d/sys_proxy.sh
    http_proxy="http://proxy-server:port"
    https_proxy="http://proxy-server:port"
    ftp_proxy="http://proxy-server:port"
    no_proxy=localhost,127.0.0.1,...
    export http_proxy ftp_proxy https_proxy no_proxy

设置静态IP:

    $ ifconfig
    # 查看网卡，ubuntu14.04 eth0, ubuntu16.04 ens160
    $ vim /etc/network/interfaces
    auto eth0
    iface eth0 inet static
        address 192.168.0.1
        netmask 255.255.255.0
        gateway 192.168.0.0
        dns-nameservers 8.8.8.8
    $ sudo service networking restart

ubuntu18:

    netplan:
    $ sudo vim /etc/netplan/*.yaml

    network:
      version: 2
      renderer: networkd
      ethernets:
        ens160:
          dhcp4: no
          addresses: [192.168.1.0/23]
          gateway4: 193.168.0.1
          nameservers:
            addresses: [8.8.8.8]

    $ sudo netplan apply

## ubuntu vlan

vlan需要内核模块8021q

    $ echo "8021q" >> /etc/modules

    auto vlan1023
    iface vlan1023 inet static
        address 18.28.4.123
        netmask 255.255.255.0
        gateway 18.28.4.1
        vlan-raw-device eth0

创建vlan

    // 创建vlan(eth0.10), attach到interface(eth0)
    auto eth0.10
    iface eth0.10 inet manual
        vlan-raw-device eth0

## ubuntu bridge

    # 需要绑定到interface才需要配置interface.
    auto eth0
    iface eth0 inet manual

    auto br0
    iface br0 inet dhcp
    # For static configuration delete or comment out the above line and uncomment the following:
    # iface br0 inet static
    #  address 192.168.1.10
    #  netmask 255.255.255.0
    #  gateway 192.168.1.1
    #  dns-nameservers 192.168.1.5
    #  dns-search example.com
    bridge_ports eth0
    # 不绑定到interface就是none
    # bridge_ports none 
    bridge_stp off
    bridge_fd 0
    bridge_maxwait 0

创建bridge

    // 创建bridge，attach到vlan.
    auto br0
    iface br0 inet static
        address 172.16.0.4
        netmask 255.255.240.0
        bridge_ports eth0.10
        bridge_stp off
        bridge_fd 0
        bridge_vlan_aware yes

## E: Sub-process /usr/bin/dpkg returned an error code (1)

method1:

    $ sudo mkdir -p /var/lib/dpkg/info/package
    $ sudo mv /var/lib/dpkg/info/package.* /var/lib/dpkg/info/package

method2:

    $ sudo apt-get purge package
    $ sudo apt-get install package

## ubuntu安装配置python3.7

ubuntu16.04自带python3.5, ubuntu18.04自带python3.6.

测试: ubuntu16.04/18.04安装python3.7

    $ sudo apt-get install software-properties-common
    $ sudo add-apt-repository ppa:deadsnakes/ppa
    $ sudo apt-get update
    $ sudo apt-get install python3.7 python3.7-gdbm python3.7-dev

    $ sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.7 1
    $ sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.7  1
    $ sudo update-alternatives --config python
    $ sudo update-alternatives --config python3

    $ sudo apt-get purge python-pip python3-pip
    $ sudo rm -rf /usr/bin/pip* /usr/local/bin/pip* /usr/lib/python3*/dist-packages/pip /usr/local/lib/python3.*/dist-packages/pip

    $ sudo apt-get install python3-pip
    $ sudo pip3 install -U pip

    // ubuntu16.04通过apt-get安装的c-binding包默认还是到python3.5.
    // ubuntu18.04 是到python3.6.
    #!/usr/bin/env bash
    for PKG in `find /usr/lib/python3/dist-packages -name "*.so"`
    do
        DIR=`dirname ${PKG}`
        OLD=`basename ${PKG}`
        NEW=`echo ${OLD} | sed 's/35m/37m/g'`
        cp -f ${PKG} ${DIR}/${NEW}
    done

***

# Centos/Fedora/Redhat安装后的基本配置

    $ sudo yum -y install epel-release kernel-devel gcc gcc-c++

## Centos网络配置

安装mini版本之后配置网络：

    $ vi /etc/sysconfig/network-scripts/ifcfg-enxxx
    ONBOOT=no -> yes
    # service network restart
    $ sudo yum install net-tools
    $ ifconfig

设置静态IP:

    $ vim /etc/sysconfig/network-scripts/ifcfg-enxxx
    ONBOOT=yes
    BOOTPROTO=static # dhcp(自动获取), static(固定IP), node(手动设置)
    PREFIX="21"
    IPADDR="192.168.0.1"
    NETMASK="255.255.255.0"
    GATEWAY="192.168.0.0"
    DNS1="192.168.0.0"
    $ sudo nmcli c reload

设置可以同时访问外网和本地连接的方法：

    # 网卡１用于外网连接
    # settings -> network -> network card1 -> NAT
    $ cat /etc/sysconfig/network-scripts/ifcfg-en01
    BOOTPROTO=dhcp # 自动获取ip
    ONBOOT=yes
    UUID # 通过$ nmcli con show 命令查看
    HWADDR # 通过 $ ip addr 命令查看, 这个可以不设置

    # 网卡２用于本地局域网
    setting -> network-> network card2 -> host-only
    # cp /etc/sysconfig/network-scripts/ifcfg-en01 /etc/sysconfig/network-scripts/ifcfg-en02
    $ vim /etc/sysconfig/network-scripts/ifcfg-en02
    NAME
    DEVICE
    BOOTPROTO=static
    ONBOOT=yes
    UUID
    HWADDR
    IPADDR=192.168.56.102

    $ sudo service network restart

修改hostname,局域网可以根据hostname相互访问：

    $ sudo vim /etc/sysconfig/network
    NETWORKING=yes
    HOSTNAME=new-hostname

    $ sudo vim /etc/hostname
    new-hostname
    $ sudo vim /etc/hosts
    ip-address hostname

    $ sudo reboot

## centos安装增强功能

    $ sudo mkdir -p /media/cdrom
    $ sudo mount /dev/cdrom /media/cdrom
    $ cd /media/cdrom
    $ sudo ./VBoxLinuxAdditions.run --nox11

