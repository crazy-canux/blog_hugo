---
title: "Builtins"
date: 2016-03-31T21:51:03
categories: ["Linux"]
tags: ["shell"]
keywords: []
author: "Canux"
draft: false
---

# Linux内置命令

内置命令在bash/builtins目录中

shell命令分为内置命令和外部命令.

查看一个命令是内置命令还是外部命令：

    type -a [command]

    提示"[command] is a shell builtin"就表示是内置命令，否则就是外部命令。

查看所有内置命令：

    help
    enable -a

查看内置命令的帮助：

    help [command]

***

    type
    enable
    help

    caller
    alias
    bg
    bind
    break
    builtin
    command
    compgen
    complete
    compopt
    continue
    declare
    disown
    let
    local
    logout
    mapfile
    popd
    printf
    pushd
    read
    readarray
    readonly
    return
    shift
    shopt
    source
    suspend
    times
    trap
    true
    typeset
    ulimit
    umask
    unalias
    unset
    wait
    eval
    exec
    exit
    export
    false
    fc
    fg
    getopts
    hash
    history
    jobs

路径相关:

    cd
    dirs
    pwd

test:

    test

set:

    set
    set -e 命令失败立即退出
    set -o pipefail 对于有管道的操作，返回最后一个非零返回值的命令的返回码.

echo: 

    echo
    echo 'origin'  # 不打印变量
    echo "_${VAR}_" # 查看变量前后是否有空格.

kill:

    kill
