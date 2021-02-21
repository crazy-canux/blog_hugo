---
title: "Nodejs"
date: 2016-09-27T03:31:25
categories: ["Web"]
tags: ["nodejs"]
keywords: []
author: "Canux"
draft: false
---

# Nodejs

<https://github.com/nodejs/node>

Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境。

Node.js 使用了一个事件驱动、非阻塞式 I/O 的模型，使其轻量又高效。

Node.js 的包管理器npm，是全球最大的开源库生态系统.

常说的javascript是前端语言，nodejs就是后端版本的javascript。

安装nodejs(npm):

    $ sudo apt-get install nodejs
    $ brew install nodejs

***

# nvm

<https://github.com/nvm-sh/nvm>

nodejs版本管理器.

install:

    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash

verify:

    command -v nvm

usage:

    // 查看所有可安装版本
    nvm ls-remote

    // 安装指定版本
    nvm install v14.15.5

    // 查看已安装版本
    nvm ls

    // 指定版本
    nvm use v14.15.5

    // 查看版本
    nvm run node --version
