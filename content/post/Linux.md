---
title: "Linux"
date: 2016-03-31T21:48:59
categories: ["Linux"]
tags: ["linux"]
keywords: []
author: "Canux"
draft: false
---

# Linux

Linux严格讲指的是Linux这一类操作系统的内核。

Linux内核的github：

<https://github.com/torvalds/linux>

Linux内核的站点：

<https://www.kernel.org/>

商业化的linux系统：

1. redhat enterprise linux
2. suse enterprise linux

免费的服务器版本：

1. centos (rhel的免费版服务器版)
2. open suse
3. debian/ubuntu

免费的桌面版本：
1. fedora (原来的redhat desktop linux)
2. open suse
3. debian/ubuntu

# Linux桌面环境

X windows

KDE

GNOME2(Mate)

GNOME3(Mate/Cinnamon)

Unity

xface

lxde

enlightenment(https://www.enlightenment.org/start)

# Linux桌面管理器：

xDM

gDM(gnome)

kDM(kde)

lightDM

# Linux编程

## Linux程序调用结构：

1. 应用程序(包括shell外部命令)/Shell命令(也就是shell内部命令)
2. C标准库glibc(包括ISO C和POSIC封装的系统系统调用)
3. Linux系统调用
4. Linux内核

## 查看手册：

查看man帮助:

    $ man man

手册章节:

1. Executable programs or shell commands
2. System calls (functions provided by the kernel)
3. Library calls (functions within program libraries)
4. Special files (usually found in /dev)
5. File formats and conventions eg /etc/passwd
6. Games
7. Miscellaneous (including macro packages and conventions), e.g. man(7), groff(7)
8. System administration commands (usually only for root)
9. Kernel routines [Non standard]

查看shell命令

    man 1 <cmd>

查到的是系统调用（实际上也是POSIX封装的同名函数）

    man 2 <name>

    // 查看系统调用所有函数宏
    man 2 syscalls
    // 查看未实现的系统调用
    man 2 unimplemented

查到的glibc（包括ISO C，POSIX的部分函数，其它库）

    man 3 <name>

查看标准：

    man 7 <name>

    // 查看glibc标准
    man 7 libc
    // 查看C和Linux的标准
    man 7 standards

# Grub

引导程序。

windows的引导程序是NTloader。

linux/unix的引导程序有lilo和grub。
