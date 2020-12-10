---
title: "Nodejs"
date: 2016-09-27T03:31:25
categories: ["JavaScript"]
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

安装nodejs:

    $ sudo apt-get install nodejs
    $ brew install nodejs

***

# npm

npm加速:

    $ npm config set registry https://registry.npm.taobao.org
    $ npm config get registry

安装:

    // 修改默认全局安装路径
    mkdir /path/npm_global
    npm config set prefix /path/npm_global
    echo 'export PATH=/path/npm_global/bin:$PATH' >> ~/.profile
    source ~/.profile
    npm install -g <pkg>

    // 安装到当前目录的 node_modules
    $ npm install <name>
    // 安装到全局的node_modules
    $ npm install -g <name>

    // 查看安装了哪些包
    $ npm list --depth=0 --global

配置:

    // 查看配置
    $ npm config ls -l

***

# module

nodejs加载的路径:

    1. 当前目录
    2. $HOME/.node_modules
       $HOME/.node_libraries
       $PREFIX/lib/node_modules
    3. $NODE_PATH

    // 查看prefix (也是-g 安装的目录)
    $ npm config ls -l | grep prefix
