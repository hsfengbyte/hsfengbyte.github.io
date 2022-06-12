---
title: Linux（一）Iproute2
tags: Linux
---

Iproute2是一个在Linux下的高级网络管理工具软件。实际上，它是通过rtnetlink sockets方式动态配置内核的一些小工具组成的，从Linux2.2内核开始，Alexey Kuznetsov 实现了通过rtnetlink sockets用来配置网络协议栈，它是一个现代的强大的接口。

### 管理IP

```shell
# 查看所有IP地址
ip a/addr/address
ip a/addr/address sh/show
# 查看指定IP地址
ip a/addr/address sh/show dev eth1
ip a/addr/address sh/show eth1
# 增加或删除IP地址
ip a/addr/address add 192.168.78.130/24 dev eth1
ip a/addr/address del/delete 192.168.78.130/24 dev eth1
# 删除eth1所有IP地址
ip a flush dev eth1
# 删除eth1的所有IPv4的IP地址
ip -4 a flush dev eth1
```

### 管理网络设备

```shell
# 查看网络设备信息
ip link sh/show/l/list/ls
ip link sh/show/l/ls/lsit eth1
ip link sh/show/l/ls/list dev eth1
# 停止与激活网络设备
ip link set dev eth1 down
ip link set dev eth1 up
```

### 管理路由

```shell
# 查看路由表
ip r/ro/route
ip r/ro/route sh/show
ip r/ro/route sh/show dev eth1
# 添加或删除路由
ip r/ro/route add 192.168.79.0/24 dev eth1
ip r/ro/route d/del/delete 192.168.79.0/24
ip r/ro/route d/del/delete 192.168.79.0/24 dev eth1
# 默认路由的删除、添加与修改
ip r/ro/route d/del/delete default
ip r/ro/route add default via 192.168.78.1
ip r/ro/route chg/change default via 192.168.78.2
```

### 管理ARP表

```shell
ip n/neigh/neighbuor sh/show
```

### 管理网桥

```shell
ip link add name bridge_name type bridge
# 也可以简化 ip link add bridge_name type bridge
ip link set bridge_name up
# 想要添加Interface到网桥上，interface状态必须是Up
ip link set eth0 up
# 添加eth0 interface到网桥上
ip link set eth0 master bridge_name
# 从网桥解绑eth0
ip link set eth0 nomaster
# eth0 可以关闭的
ip link set eth0 down
# 删除网桥可以用
ip link delete bridge_name type bridge
# 也可以简化为
ip link del bridge_name
```
