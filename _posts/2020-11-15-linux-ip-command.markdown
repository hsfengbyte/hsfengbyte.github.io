---
layout: post
title: Linux ip command
---

查看所有IP地址

```shell
ip a/addr/address
ip a/addr/address sh/show
```

查看指定IP地址

```shell
ip a/addr/address sh/show dev eth1
ip a/addr/address sh/show eth1
```

增加或删除IP地址

```shell
ip a/addr/address add 192.168.78.130/24 dev eth1
ip a/addr/address del/delete 192.168.78.130/24 dev eth1
```

删除eth1所有IP地址

```shell
ip a flush dev eth1
```

删除eth1的所有IPv4的IP地址

```shell
ip -4 a flush dev eth1
```

查看网络设备信息

```shell
ip link sh/show/l/list/ls
ip link sh/show/l/ls/lsit eth1
ip link sh/show/l/ls/list dev eth1
```

停止与激活网络设备

```shell
ip link set dev eth1 down
ip link set dev eth1 up
```

查看路由表

```shell
ip r/ro/route
ip r/ro/route sh/show
ip r/ro/route sh/show dev eth1
```

添加或删除路由

```shell
ip r/ro/route add 192.168.79.0/24 dev eth1
ip r/ro/route d/del/delete 192.168.79.0/24
ip r/ro/route d/del/delete 192.168.79.0/24 dev eth1
```

默认路由的删除、添加与修改

```shell
ip r/ro/route d/del/delete default
ip r/ro/route add default via 192.168.78.1
ip r/ro/route chg/change default via 192.168.78.2
```

查看ARP表

```shell
ip n/neigh/neighbuor sh/show
```