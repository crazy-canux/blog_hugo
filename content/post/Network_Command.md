---
title: "Command"
date: 2019-03-31T21:51:25
categories: ["Network"]
tags: ["command"]
keywords: []
author: "Canux"
draft: false
---

# telnet

    telnet

# nc/netcat

    // 有的系统默认安装netcat-traditional
    $ sudo apt-get install netcat-traditional 
    // 安装netcat-openbsd
    $ sudo apt-get install netcat-openbsd
    // 设置默认值
    $ sudo update-alternatives --config nc
    $ sudo update-alternatives --set nc /bin/nc.openbsd

    nc -z IP PORT # 查看指定tcp://ip:port是否监听
    nc -zu IP PORT # 查看udp://ip:port是否监听

# ping

    $ sudo apt-get install iputils-ping
    ping # 用于确定网络的连通性

# ifconfig

    $ sudo apt-get install net-tools
    ifconfig # 查看up的interface
    ifconfig -a  # 查看所有的interface
    ifconfig <bridge>/<interface> up/down

# brctl

    $ sudo apt-get install bridge-utils
    brctl show
    brctl addbr <bridge> # 添加bridge
    brctl delbr <bridge> # 删除bridge
    brctl addif <bridge> <interface> # 绑定interface到bridge
    brctl delif <bridge> <interface> # 删除bridge上的interface

# route

    route # 操作路由表的命令
    route -n

# ip

    $ sudo apt-get install iproute2

link:

    ip link show

    # 创建vlan
    ip link add link eth1 name eth1.10 type vlan id 10
    # 添加ip
    ip addr add 192.168.100.1/24 brd 192.168.100.255 dev eth1.10
    # 启动vlan
    ip link set dev eth1.10 up
    # 关闭vlan
    ip link set dev eth1.10 down
    # 删除vlan
    ip link delete eth1.10

addr/address

    ip addr show

route

    ip route show

rule

    ip rule show

# arp

    arp # 用于确定IP地址的网卡物理地址

# dig

# nslookup

    nslookup # 查询IP地址和对应的域名

# ethtool

    ethtool # 查询网络设备信息

# netstat

    netstat
    -a, --all, --listening # 显示所有socket, 默认只显示connected
    -l, --listening  # 显示listening
    -n, --numeric
    -p, --programs # 显示pid或程序名称
    # socket选项:
    -t, --tcp
    -u, --udp
    -w, --raw
    -x, --unix
    --ax25
    --ipx
    --netrom

    # 常用
    netstat -anp    # 查看哪些端口是打开的．
    sudo netstat -anp | grep port # 查看端口是否被使用
    sudo netstat -tulnp # 查看tcp&udp端口是否被监听

# iftop

查看网络流量

    # <http://www.ex-parrot.com/~pdw/iftop/>
    $ sudo apt-get install iftop
    $ sudo iftop

# tcpdump

    tcpdump

    tcpdump udp port <port> 抓udp 在port端口的包
    tcpdump -i <interface> host <ip>

# traceroute

# wget

    wget [option] [URL]

    -a, --append-output=FILE 输出重定向到日志
    -o, --output-file=FILE
    -q, --quiet    不输出
    -b, --background
    -nv, --no-verbose
    --header "authorization:c2FuZGJveDpTMG5pY3dhbGwK"

    -t, --tries=NUMBER    超时重连次数, 0表示不限制, 默认20
    -nc, --no-clobber    不覆盖原有文件
    -N, --timestamping   只下载比本地新的文件
    -c, --continue    断点续传,会覆盖-N
    -T, --timeout=SECONDS    超时时间, 默认900s
    -w, --wait=SECONDS    重连之间的等待时间
    -O, --output-document=FILE, 重命名下载文件

    -nH, --no-host-directories 不创建站点的根目录
    -x, --force-directories    创建和服务器一样的结构下载
    -P, --directory-prefix=PREFIX  指定下载的目录

    -R,  --reject=LIST 排除下载的文件
    -r, --recursive  迭代下载
    -np, --no-parent 不下载父目录的内容

    # 同步目录
    wget -Nc -r -np -nH --cut-dirs=3 -R "index.*, *.js, *.css, *.html, *.jpg, *.png, *.gif" -P /path/to/source/ http://host/path/to/dest/

    # ssl + basic auth
    wget --no-check-certificate --user <user> --password <pw> -nv -N -P <dest-folder> <src-url>

    $ wget -q -N -P /folder url
    $ wget -q -c -O /path/to/file.ext url

# curl

    curl
    -X/--request post/patch/delte/get/...
    -H/--header 'content-type: application/json'
    -d/--data '{"key": "value"}'
    --data-raw DATA  HTTP POST data, '@' allowed (H)
    --data-ascii DATA  HTTP POST ASCII data (H)
    --data-binary DATA  HTTP POST binary data (H)
    --data-urlencode DATA  HTTP POST data url encoded (H)
    -G, --get           Send the -d data with a HTTP GET (H)
    -k/--insecure    ignore ssl check.
    -u/--user <user:password>
    -s/--silent        Silent mode
