---
title: "BitBake"
date: 2020-07-12T21:18:33
categories: ["DevOps"]
tags: ["bitbake"]
keywords: ["yocto", "openembedded"]
author: "Canux"
draft: false
---

# BitBake

bitbake是类似于make的构建工具，主要用于OpenEmbedded和yocto构建linux发行版.

<https://github.com/openembedded/bitbake>

bitbake:

    $ bitbake python -c devshell/devpyshell
    $ bitbake python -c clean/cleanall/cleanstate
    $ bitbake python -c compile
    $ bitbake python -c fetch/fetchall

    # 查找下载地址
    $ bitbake -e python | grep ^SRC_URI

    # 根据文件查找包名
    bitbake> oe-pkgdata-util find-path /usr/bin/python3

    # 包名查找recipe
    bitbake> oe-pkgdata-util lookup-recipe python3-core

# openembedded

<https://github.com/openembedded/openembedded-core>

# yocto(poky)

poky是一个开源的最小构建示例，内置bitbake，可直接编译.

<http://git.yoctoproject.org/cgit/cgit.cgi/poky/log/>

<http://git.yoctoproject.org/cgit/cgit.cgi/meta-virtualization/tree/recipes-containers>

<https://github.com/crazy-canux/poky>

# toaster