---
title: "Devops"
Date: 2018-01-01T10:49:21
categories: ["Golang"]
tags: ["devops"]
keywords: []
author: "Canux"
draft: false
---

# Go

go有三种安装方式：

1. 源码安装
2. 标准包安装
3. 第三方工具安装

GOROOT:

    GOROOT 就是go的安装目录

windows标准包安装go:

    下载zip包解压到C:\go
    %GOROOT%=C:\go

linux标准包安装go:

    下载.tar.gz包解压到/usr/local/go
    export GOROOT=/usr/local/go
    export PATH=$PATH:$GOROOT/bin

验证安装：

    $ go --help
    $ go version

第三方工具gvm安装go:

<http://github.com/moovweb/gvm>

    $ gvm install go1.9.2
    $ gvm use go1.9.2

***

# GOPATH

gopath用来存放go源码，go的可运行文件，以及相应的编译之后的包文件．

GOPATH 从go1.1到1.7都需要设置，而且不能是go的安装目录, go1.8开始有默认值:

    GOPATH=$USERPROFILE%go
    GOPATH=$HOME/go

gopath结构：

    src    存放源码
    pkg    编译后的库文件
    bin    编译后生成的可执行文件

bin目录一般加入环境变量:

    $GOPATH/bin

gopath有多个值时用冒号分开即可.

***

# go命令

    $ go help [command]

get

下载并安装包和依赖, 也就是安装第三方的库．

    $ go get [...] [packages]

    > go get安装第三方包如果出现依赖无法安装，可以通过github下载．
    $ cd golang.org/x
    $ git clone https://github.com/golang/crypto.git crypto
    $ go install golang.org/x/crypto/ssh

    go get -u (without any arguments) now only upgrades the direct and indirect dependencies of your current package, and no longer examines your entire module.

    go get -u ./... from your module root upgrades all the direct and indirect dependencies of your module, and now excludes test dependencies.

    go get -u -t ./... is similar, but also upgrades test dependencies.

build

编译包和依赖

    $ go build [-o output] [-i] [build flags] [packages]

install

编译并安装包和依赖

    $ go install [build flags] [packages]

run

编译并运行程序

    $ go run [...] gofiles... [...]

fmt

格式化代码和文档：

    $ go fmt [...] [packages]

vet

检测代码常见错误:

    $ go vet [-n] [-x] [build flags] [vet flags] [packages]

test

测试包:

    $ go test [...] [packages] [...]

doc

查看文档：

    $ go doc [package/symbol]

mod

    go mod init <name>
    go mod tidy
    go mod download
    go mod verify

env

    // 通过env 设置golang的变量，取代系统环境变量
    go env
    go env -w GOPATH="$HOME/Src/go"
    go env -w GOBIN="/usr/local/go/bin"
    go env -w GOROOT="/usr/local/go"

clean

    go clean -cache
    go clean -modcache

***

# 安装第三方包

go get的功能很有限．

godep和golide都会被官方的dep取代．

## dep(deprecated)

<https://github.com/golang/dep>

无法解决GFW的问题.

安装：

    $ go get -u github.com/golang/dep/cmd/dep

go官方包管理器

    # 初始化一个使用dep管理包的项目，
    # 创建Gopkg.toml, Gopkg.lock, vendor/
    $ dep init

    $ dep status

    $ dep prune

    $ dep ensure
    $ dep ensure -update

## module

vgo已经集成到go1.11

通过go mod init初始化两个文件.

go.mod:

    module
    go
    require
    exclude
    replace

    // 使用本地module,或使用指定repo里面的module
    replace github.com/crazy-canux/go-devops => /path/to/local/github.com/crazy-canux/go-devops


go.sum:

    go env -w GOSUMDB=off

设置相关环境变量:

    GO111MODULE:
    // auto/on/off
    go env -w GO111MODULE=on

    GOPROXY:
    go env -w GOPROXY=https://goproxy.cn,direct
    https://proxy.golang.org //默认值
    https://goproxy.cn
    https://mirrors.aliyun.com/goproxy/

    GOSUMDB:

    GOPRIVATE

    GONOPROXY

    GONOSUMDB

***

# 项目结构

没有子目录包结构：

    go-devops
        |- README.md
        |- doc.go
        |- grafana.go
        |- grafana_test.go
        ...

    import "go-devops"

有子目录的包结构：

    go-devops
        |- README.md
        |- doc.go // package go_devops
        |- grafana
           |- doc.go // package grafana
           |- grafana.go
           |- grafana_test.go

    import "go-devops/grafana"

***

# proxy

goproxy.io

<https://github.com/goproxyio/goproxy>

goproxy.cn

<https://github.com/goproxy/goproxy.cn>
