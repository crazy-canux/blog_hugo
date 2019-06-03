---
title: "Virtualbox"
date: 2019-06-05T21:47:54
categories: ["Virtualization"]
tags: ["virtualbox"]
keywords: []
author: "Canux"
draft: false
---

# Virtualbox


# vboxmanage

vm

    $ vboxmanage import win7.ova // 导入ova

    // 修改虚拟机的网络为hostonly
    $ vboxmanage modifyvm "win764" --nic1 hostonly --hostonlyadapter1 vboxnet0

    $ vboxmanage modifyvm <vm> --name <new-name> // 重命名vm

    // 启动虚拟机
    $ vboxmanage startvm "Win732"
    $ vboxmanage startvm <vm> --type headless 
    $ VBoxHeadless --startvm <uuid|name> --vrde on

    $ vboxmanage list vms/runningvms // 查看所有/运行虚拟机

snapshot

    //查看快照
    $vboxmanage <vm/uuid> snapshot list 

    $ vboxmanage snapshot "Win732" take <name> --live --pause // 创建快照

    $ vboxmanage snapshot delete <snapshot-name/uuid> // 删除快照

hostonly-network

    $ vboxmanage hostonlyif crate // 创建hostonly bridge
    $ vboxmanage hostonlyif ipconfig vboxnet0 --ip 192.168.1.1 // 给hostonly bridge分配ip

    // 查看所有hostonly网路
    $ vboxmanage list hostonlyifs

bridge-network

nat-network

storage

extpack

    // 安装扩展包
    $ sudo vboxmanage extpack install Oracle_VM_VirtualBox_Extension_Pack-6.0.8.vbox-extpack