---
title: "Find"
date: 2016-04-20T13:55:36+08:00
categories: ["Linux"]
tags: ["shell"]
keywords: []
author: "Canux"
draft: false
---

# find

Find     查找，用于在目录中查找。
find  path  options  tests  actions
path 路径
options 选项
tests 测试
actions 动作

optioins选项:
-follow
-depth
-maxdepth
-mindepth
find   dir  -mindepth  n     指定最小的目录深度，至少从dir往下n级目录开始往下搜索，dir和n级之间的忽略。
find   dir   -maxdepth  n      指定最大目录深度，不搜索n级之后的目录。

test选项很多：
-newer   pattern   比pattern文件要新
-user    pattern      文件属主是pattern
-name   pattern    查找和type匹配的
-iname  pattern    查找和type匹配的，会忽略大小写
-iwholename
-path   pattern     按照文件路径匹配
-type   c           c是文件类型，按照文件类型匹配文件
-size   +/-    nk/c/w/k/b/M/G    匹配大于或小于n  kb/..  的文件
-perm   XXX       基于文件权限的匹配
find dir  –atime/mtime/ctime    +/-n    根据时间累匹配，atime表示访问时间，mtime表示修改时间，ctime表示变化时间，+表示大于，-表示小于，单位是天。
-a/-and   pattern
-o/-or     pattern
！/-not    pattern
find  dir  !  test   pattern   列出所有没有按照-options  pattern模式的项
\(...\)    使用括号需要用引号来引用。

action选项：
-prune     如果是一个指定的目录就忽略这个目录,要用-path指定目录.
-print          打印，换行符结尾,所有结果一行一个。
-print0        打印，空字符结尾，所有结果打印到一行。
-delete       删除
-exec   command     执行一个命令
exec   command   {}   \;

operators: find可以用一些运算符来连接多个test条件。
！expr   #取反
expr1    -a    expr2    #与运算，可以省略-a
expr1    -o    expr2    #或运算

查找当前目录下除了develop里面的文件以外的30天之内修改过的.txt文件
find .   -path ./develop -prune  -o  -mtime -30  -type f  -name  "*.txt"   -print

将前面的命令的结果通过管道和xargs作为后面命令的输入，类似于find命令的-exec选项。
格式 ：command1   |   xargs   -options   command2
Xargs   -n    number    设置每行显示的参数数量为number。
Xargs  -d     char   指定char为界定符，也就是将char换成空格。
Xargs  -I   {}    将命令参数用STDIN的参数替换掉。
Xargs   -0       以/0为定界符，而不是空格。

find . -name “*.c” | xargs wc -l
find . -name “*.c” -exec wc -l {}   \;
wc -l  `find . -name “*.c”`

