---
title: "Image"
date: 2019-12-03T21:50:47+08:00
categories: ["Container"]
tags: ["docker"]
keywords: []
author: "Canux"
draft: false
---

# image

容器镜像

# scratch

scratch是空白镜像，一般用于基础镜像构建.比如制作alpine/ubuntu/debian/busybox镜像.

# ubuntu/debian

Hash Sum mismatch:

    RUN set -ex \
    && apt-get clean \
    && apt-get update -o Acquire::CompressionTypes::Order::=gz \
    && apt-get update \
    && apt-get install -y --allow-unauthenticated --no-install-recommends \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

# centos/fedora

# buildpack-deps

在ubuntu/debian基础上安装一些工具，比ubuntu/debian镜像更大.

<https://github.com/docker-library/buildpack-deps>

# busybox

<https://github.com/docker-library/busybox>

# alpine(推荐)

很多语言的都是基于alpine: python-version:alpine-version, golang-version:alpine-version.

一个基于musl和busybox的linux发行版.

<https://www.alpinelinux.org/>

alpine比distroless尺寸大，包含包管理和shell，方便调试.

alpine使用musl代替glibc会导致有的程序无法运行, 解决:

    # mkdir /lib64
    # ln -s /lib/libc.musl-x86_64.so.1 /lib64/ld-linux-x86-64.so.2

注意wget 部分参数不可用.

# distroless

google提供的只包含运行时的精简镜像.

缺点是没有包管理和shell，不方便调试.

<https://github.com/GoogleContainerTools/distroless>

# slim

减小image大小,适用于web程序.

<https://github.com/docker-slim/docker-slim

***

