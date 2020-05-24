---
title: "Jenkins"
date: 2016-04-15T09:41:39
categories: ["DevOps"]
tags: ["jenkins"]
keywords: []
author: "Canux"
draft: false
---

# Jenkins

Jenkins is a self-contained, open source automation server which can be used to automate all sorts of tasks such as building, testing, and deploying software.

<https://github.com/jenkinsci/jenkins>

安装好Jenkins后安装需要的插件．

安装jenkins:

    # download jenkins.war and install java8.
    $ java -jar jenkins.war --httpPort=8080
    $ firefox http://localhost:8080

设置开机自动启动，不用每次从终端启动:

    $ vim /etc/systemd/system/jenkins.service
    $ systemctl daemon-reload
    $ systemctl enable jenkins.service
    $ systemctl start jenkins

jinkens主目录:

    # 默认主目录在/home/canux/.jenkins

***

# nodes

添加节点需要安装和master版本一样的java.

on jenkins agent:

1. install java.
2. create folder and grant permission. (/home/jenkins)

on jenkins:

1. create credential and node.

***

# plugins

## thinbackup

备份插件，主要备份jenkins主目录.

## pipeline

<https://github.com/jenkinsci/pipeline-plugin>

使用pipeline需要先安装pipeline插件.

pipeline是groovy语法的jenkins的DSL.

## blueocean

<https://github.com/jenkinsci/blueocean-plugin>

# git plugin

<https://github.com/jenkinsci/git-plugin>

# docker plugin

<https://github.com/jenkinsci/docker-workflow-plugin>

<https://docs.cloudbees.com/docs/admin-resources/latest/plugins/docker-workflow>

# k8s

<https://github.com/jenkinsci/kubernetes-plugin>

***

# docker

在docker上执行流水线

    agent {
        docker {
            image 'maven:3-alpine'
            label 'my-defined-label'
            args  '-v /tmp:/tmp'
        }
    }

    agent {
        // Equivalent to "docker build -f Dockerfile.build --build-arg version=1.0.2 ./build/
        dockerfile {
            filename 'Dockerfile.build'
            dir 'build'
            label 'my-defined-label'
            additionalBuildArgs  '--build-arg version=1.0.2'
        }
    }