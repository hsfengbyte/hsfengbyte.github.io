---
title: QEMU 网络模式
tags: Qemu
---

QEMU 支持多种网络模式，简单介绍如何使用这几种网络模式。

<!--more-->

### 一、QEMU支持的网络模式

qemu-kvm主要向客户机提供了4种不同模式的网络。

1)基于网桥(bridge)的虚拟网卡；

2)基于NAT的虚拟网络（网络地址转换NAT）；

3)QEMU内置的用户模式网络(user mode networking)；

4)直接分配网络设备的网络(包括VT-d和SR-IOV)

### 二、基于网桥(bridge)的虚拟网卡

建立一个bridge,将其绑定到一个可正常工作的网络接口上，同时让bridge成为连接本机与外部网络的接口。

```shell
brctl addr br0
brctl addif br0 eth0
brctl stp br0 on
```

建立bridge后的状态是让网络接口eth0进入混杂模式(promiscuous mode，接收网络中所有数据包)，网桥br0进入转发状态(forwarding state)，并且与eth0具有相同的MAC地址.

建立一个TAP设备，将其绑定到网桥上

```shell
tunctl -u $(whoami) -t tap0
brctl addif br0 tap0
```

使用TAP设备

```shell
qemu -net nic -net tap,ifname=tap0
```

QEMU网络启动关闭脚本

客户机启动网络前会执行的脚本由"script"选项配置的(默认为/etc/qemu-ifup)。一般在该脚本上创建一个TAP设备并将其中与bridge绑定起来。

准备qemu-ifup和qemu-ifdown脚本

```shell
# cat /etc/qemu-ifup
#!/bin/bash

switch=br0

if [ -n "$1" ];then
  #tunctl -u $(whoami) -t $1 #一些较旧版本中不会自动创建TAP设备
  ifconfig $1 up
  sleep 0.5s
  brctl addif $switch $1
  exit 0
else
  echo 'Error: no specifed interface.'
  exit 1
fi
```

由于qemu-kvm工具在客户机关闭时会解除TAP设备的bridge绑定,也会自动删除已不再使用的TAP设备，所有qemu-ifdown这个脚本不是必需的,最好设置为downscript=no(qemu-kvm的script参数是虚拟机启动时执行的脚本,downscript参数是虚拟机关闭时执行的参数)

```shell
# cat /etc/qemu-ifdown
#!/bin/bash

switch=br0

if [ -n $1 ];then
  tunctl -d $1
  brctl delif ${switch} $1
  ip link set $1 down
  exit 0
else
  echo "Error: no interface specified"
  exit 1
fi
```

### 三、基于NAT的虚拟网络（网络地址转换NAT）

网络设备，虚拟机连接主机

主机网卡：**enp0s3**

TAP设备：**tap0**

初始化TAP设备

```shell
tunctl -u $(whoami) -t tap0
ip addr add 192.168.0.1/24 dev tap0
ip link set dev tap0 up
```

虚拟机的启动命令行增加网络参数

```shell
-net nic -net tap,ifname=tap0
```

设置虚拟机IP，此处enp0s3为虚拟机网卡

```shell
ip addr add 192.168.0.2/24 dev enp0s3
```

虚拟机通过主机连接internet，现在实现了虚拟机和host的联网，如果需要虚拟机连接internet，则要在host设置NAT：

```shell
echo 1 > /proc/sys/net/ipv4/ip_forward
iptables -t nat -A POSTROUTING -o enp0s3 -s 192.168.0.0/24 -j MASQUERADE
```

虚拟机添加DNS服务器

```shell
# /etc/resolv.conf
...
namserver 8.8.8.8
```


### 四、内置的用户模式网络(user mode networking)

虚拟机的启动命令行增加网络参数

```shell
-net nic -net user
```

### 五、仅主机(Host-only)模式

建立一个TAP设备，将其绑定到网桥上

```shell
tunctl -u $(whoami) -t tap0
brctl addif br0 tap0
```

使用TAP设备

```shell
qemu -net nic -net tap,ifname=tap0
```

### 六、桥接网络

> 重点注意：模拟主机网卡桥接模式，且混杂模式为全部允许

配置qemu虚拟机与主机在同一网络（主机所在网段）下，且可对外访问（一般为互联网），可以使用脚本1或脚本2。

> 脚本1

```shell
# net-bridge.sh
# network card 1: enp0s3
# network card 2: enp0s8
# network bridge: br0
# tap device: tap0

sysctl -w net.ipv4.ip_forwoard=1

ifconfig enp0s8 down
brctl addbr br0
brctl addif br0 enp0s8
brctl stp br0 off
brctl setfd br0 1
brctl sethello br0 1
ifconfig br0 0.0.0.0 promisc up
ifconfig enp0s8 0.0.0.0 promisc up

tunctl -t tap0 -u hsfengbyte
ip link set dev tap0 down
brctl addif br0 tap0
ifconfig tap0 0.0.0.0 promisc up
```

> 脚本2

```shell
# net-iproutes-bridge.sh
# network card 1: enp0s3
# network card 2: enp0s8
# network bridge: br0
# tap device: tap0

#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

ip link add name br0 type bridge
ip link set dev enp0s8 master br0
ip link set dev br0 up promisc on
ip link set dev enp0s8 up promisc on

ip tuntap add dev tap0 mode tap user hsfeng
ip link set dev tap0 master br0
ip link set dev tap0 up promisc on

dhclient
# dhcpcd
```

运行脚本1或脚本2后，主机网络：

```shell
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:06:c3:ec brd ff:ff:ff:ff:ff:ff
    inet 192.168.8.209/24 brd 192.168.8.255 scope global dynamic enp0s3
       valid_lft 42100sec preferred_lft 42100sec
    inet 192.168.8.208/24 brd 192.168.8.255 scope global secondary noprefixroute enp0s3
       valid_lft forever preferred_lft forever
    inet6 fe80::aa8:7bf2:99b:fcf2/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
3: enp0s8: <BROADCAST,MULTICAST,PROMISC,UP,LOWER_UP> mtu 1500 qdisc fq_codel master br0 state UP group default qlen 1000
    link/ether 08:00:27:08:4b:8c brd ff:ff:ff:ff:ff:ff
    inet 192.168.8.143/24 brd 192.168.8.255 scope global dynamic enp0s8
       valid_lft 41193sec preferred_lft 41193sec
    inet 192.168.8.142/24 brd 192.168.8.255 scope global secondary noprefixroute enp0s8
       valid_lft forever preferred_lft forever
    inet6 fe80::cfe0:a989:e6f0:a482/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
5: br0: <BROADCAST,MULTICAST,PROMISC,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 08:00:27:08:4b:8c brd ff:ff:ff:ff:ff:ff
    inet 192.168.8.143/24 brd 192.168.8.255 scope global dynamic br0
       valid_lft 42100sec preferred_lft 42100sec
    inet 192.168.8.142/24 brd 192.168.8.255 scope global secondary noprefixroute br0
       valid_lft forever preferred_lft forever
    inet6 fe80::5c79:b0f1:69eb:54d8/64 scope link
       valid_lft forever preferred_lft forever
6: tap0: <NO-CARRIER,BROADCAST,MULTICAST,PROMISC,UP> mtu 1500 qdisc fq_codel state DOWN group default qlen 1000
    link/ether aa:dd:8b:c9:3c:55 brd ff:ff:ff:ff:ff:ff
    inet 169.254.7.232/16 brd 169.254.255.255 scope link tap0:avahi
       valid_lft forever preferred_lft forever
```

虚拟机网络：

```shell
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:06:c3:ec brd ff:ff:ff:ff:ff:ff
    inet 192.168.8.208/24 brd 192.168.8.255 scope global noprefixroute enp0s3
       valid_lft forever preferred_lft forever
    inet6 fe80::aa8:7bf2:99b:fcf2/64 scope link
       valid_lft forever preferred_lft forever

```

### 七、其他问题

#### 自动获取dhcp

```shell
sudo apt intall dhcpcd
sudo systemctl enable dhcpcd
sudo systemctl start dhcpcd
sudo dhcpcd #获取ip
sudo dhcpcd -k #清空ip
```
