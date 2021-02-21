---
title: "NPM"
date: 2016-09-27T03:31:25
categories: ["Web"]
tags: ["npm"]
keywords: []
author: "Canux"
draft: false
---

# NPM

npm: node package manager.

npm 由三部分组成：
* website <https://www.npmjs.com>
* CLI
* registry <https://registry.npmjs.org>

config:

    // 修改registry
    $ npm config set registry https://registry.npm.taobao.org
    $ npm config get registry

    // 修改默认全局安装路径
    mkdir /path/npm_global
    npm config set prefix /path/npm_global
    echo 'export PATH=/path/npm_global/bin:$PATH' >> ~/.profile
    source ~/.profile
    
    // 查看配置
    $ npm config ls -l
    
install:

    // 根据当前目录package.json安装
    $ npm install

    // 安装pkg到当前目录的 node_modules
    $ npm install <pkg>
    
    // 安装到全局的node_modules
    $ npm install -g <name>
    
    --save-dev // 安装并自动更新到package.json的devDependencies.
    --no-package-lock

list:

    // 查看安装了哪些包
    $ npm list --depth=0 --global
    
init:

    // 创建package.json
    $ npm init
    
test:

    $ npm test

***

# 配置

## folders

nodejs加载的路径:

    1. 当前目录
    2. $HOME/.node_modules
       $HOME/.node_libraries
       $PREFIX/lib/node_modules
    3. $NODE_PATH

    // 查看prefix (也是-g 安装的目录)
    $ npm config ls -l | grep prefix
    
## npmrc

## package.json

package.json

package-locks

package-lock.json

## shrinkwrap.json
    
***


***


