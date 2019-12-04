---
title: "Sed"
date: 2016-04-02T11:15:57
categories: ["Linux"]
tags: ["shell"]
keywords: []
author: "Canux"
draft: false
---

# sed

sed：stream editor  流编辑器 ，主要用于文本处理。

sed命令格式：

    sed   -options   ’expression1;expression2’     file           执行多个命令
    sed   -options   [-e ‘expression1’] [-e ‘expression2’]  file  执行多个命令
    sed   -options   [-f   scriptfile]   file                     通过脚本执行命令
    sed   -options   [-f   scriptfile]   file  >  newfile  #sed修改后重定向到新文件。

sed指令需要用单引号包围。使用双引号“”可以传递变量。
Sed默认并没有修改文件file中的数据。
sed默认在stdout输出文件的所有行。
Sed地址需要使用/address/来包围。
sed使用正则表达式可以用\BRE\包围，如果模式包含/，那么可以使用除了换行符之外的所有字符包围。

options：

    -e：指定多个命令或脚本
    -f：指定执行命令的脚本
    -n：阻止自动输出，p可以打印匹配的行。
    -i: 直接修改读入的文件的内容.

experssion：

    expression：指令由模式和过程组成。
    [address]/[line-address][!]command[arguments]
    [address]表示地址，一般用模式进行寻址，address缺省表示整个文件寻址，两个地址用，隔开。
    [line-address]表示只能是一个地址。
    [!] 表示不匹配该地址的所有行

Command:

sed有25个命令。

使用大括号{}在一个地址中做嵌套操作：例如：

    /address1/,/address2/{
        /^$/d
        s/string1/string2/
        …
    }             // 单独一行，后面不能有空格

s命令：替换，替换模式空间中的行。

    sed '[address]s/oldpattern/newpattern/[flag]' filename

flag:

    n:替换每个寻址行的第n个匹配模式。默认n=1. n在1-512之间。
    g:替换每个寻址行的所有匹配模式。
    p:打印模式空间的内容
    w file：如果发生替换就将这一行写入file。只写入替换的行,不写入其它行.

用反斜杠\转义换行符：

因为反斜杠在newpattern中也用于包含换行符。

    // 将匹配的项替换成两个换行符。
    sed '
    s/pattern/\ (换行）
    \           （换行）
    string/' filename
    等价于：
    sed 's/pattern/\n\n/' filename

用反斜杠\转义与符号&：

    // 结果是： string1 pattern string2 
    sed 's/pattern/string1 & string2/g' filename

如果不转义&匹配整个pattern:

    // 结果是：string1 & string2
    sed 's/pattern/string1 \& string2/g' filename

用反斜杠\转义\n:

    // 将pattern中匹配到的第一个字串回调到newpattern中使用。
    sed 's/pattern/\1/' filename

d命令：删除，删除模式空间中的行，并不删除文件中的行。

    '[address]d'
    '1d' 删除第一行
    '$d' 删除最后一行
    ’/^$/d' 删除空行
    ‘/^\s\+$/d’  删除空白行（没有数字字符）
    ‘/^\s*$/d’ 删除空白行（没有数字字符）

a/i/c命令：追加/插入/更改.

    // 在匹配到的行下面添加追加的内容
    '[line-address]a string' filename
    '[line-address]a\
    string1\
    string2\
    string3' filename   （追加三行）

i:同上，在匹配到的行上面插入内容。

    // 将匹配到的行替换掉。
    '[address]c string' filename
    '[address]c\
    string1\
    string2\
    string3' filename

l：列表命令

    sed '[address]l' filename   打印模式空间内容，将非打印字符显示为ASCII码

p：打印命令

    sed '[address]p' filename   打印模式空间内容

=：打印行号

    sed -n '[line-address]='  filename  只打印行号

n：下一步

    sed '[address]n' filename

q:退出命令

    // 一旦找到和line-address匹配的行，脚本立即退出。
    sed '[line-address]q' filename

r/w:读/写命令

    // 读file文件追加在匹配到的行后面。
    sed '[line-address]r file'   filename

    // 将匹配到的行写入到file文件中
    sed '[address]w file'  filename

    sed –I 's/.*/\L&/g' urfile   全部转换成小写
    sed  -I 's/.*/\U&/g' urfile 全部转换成大写。
    sed   ‘/pattern/{{n; p;}}’   urfile   读取pattern下一行并打印。

n：追加下一个输入行到匹配后面并在两者间嵌入新行，改变行号。

p：打印匹配的第一行。

# 常用

    sed   ‘$a   string’   filenames           #批量往文件最后一行添加内容
    sed   ‘$i    string’   filenames           #批量往文件倒数第二行添加内容
    string里面有空格用\开头。