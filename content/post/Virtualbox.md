---
title: "Virtualbox"
date: 2019-06-03T22:53:23+08:00
categories: ["Virtualization"]
tags: ["virtualbox"]
keywords: []
author: "Canux"
draft: false
---

# Virtualbox

虚拟化工具virtualbox.

# vboxmanage

vm

    // 导入ova
    $ vboxmanage import win7.ova 

    // 添加host网络
    $ vboxmanage modifyvm "win764" --nic1 hostonly --hostonlyadapter1 vboxnet0
    // 添加bridge网络
    $ vboxmanage modifyvm "Win732" --nic2 bridged --bridgeadapter2 docker_gwbridge
    // 重命名vm
    $ vboxmanage modifyvm <vm> --name <new-name> 
    // 修改参数
    $ vboxmanage modifyvm <vm> --memory 4096 --cpus 4 --hwvirtex on --ioapic on

    // 启动虚拟机
    $ vboxmanage startvm "Win732"
    $ vboxmanage startvm <vm> --type headless 
    $ VBoxHeadless --startvm <uuid|name> --vrde on

    // 控制虚拟机
    $ vboxmanage controlvm <vm> pause/resume/reset/poweroff/savestate

    // 查看所有/运行虚拟机
    $ vboxmanage list vms/runningvms 

    // 删除vm
    $ vboxmanage unregistervm <vm> --delete

media

    // 列出所有hdd
    $ vboxmanage list hdds

    // 删除hdd
    $ vboxmanage closemedium disk <uuid> --delete

snapshot

    //查看快照
    $ vboxmanage snapshot <vm> list 

    $ vboxmanage snapshot <vm> take <name> --live --pause // 创建快照

    $ vboxmanage snapshot <vm> delete <snapshot-name/uuid> // 删除快照

hostonly-network

    $ vboxmanage hostonlyif crate // 创建hostonly bridge
    $ vboxmanage hostonlyif ipconfig vboxnet0 --ip 192.168.1.1 --netmask 255.255.255.0 // 给hostonly bridge分配ip和netmask.
    $ vboxmanage hostonlyif remove <name>

    // 查看所有hostonly网路
    $ vboxmanage list hostonlyifs

bridge-network

nat-network

storage

extpack

    // 安装扩展包
    $ sudo vboxmanage extpack install Oracle_VM_VirtualBox_Extension_Pack-6.0.8.vbox-extpack
    $ sudo vboxmanage extpack uninstall \
    "Oracle VM VirtualBox Extension Pack"
