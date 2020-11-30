---
layout: post
title: QEMU网络模式-bridge
---

**QEMU支持的网络模式**

qemu-kvm主要向客户机提供了4种不同模式的网络。

1)基于网桥(bridge)的虚拟网卡；

2)基于NAT的虚拟网络；

3)QEMU内置的用户模式网络(user mode networking)；

4)直接分配网络设备的网络(包括VT-d和SR-IOV)

**网络模式bridge**

**建立一个bridge,将其绑定到一个可正常工作的网络接口上，同时让bridge成为连接本机与外部网络的接口。**

```shell
brctl addr br0
brctl addif br0 eth0
brctl stp br0 on
```

建立bridge后的状态是让网络接口eth0进入混杂模式(promiscuous mode，接收网络中所有数据包)，网桥br0进入转发状态(forwarding state)，并且与eth0具有相同的MAC地址.

**建立一个TAP设备，将其绑定到网桥上**

```shell
tunctl -u $(whoami) -t tap0
```

使用TAP设备

```shell
qemu -net nic -net tap,ifname=tap0
```

**QEMU网络启动关闭脚本**

**准备qemu-ifup和qemu-ifdown脚本**

客户机启动网络前会执行的脚本由"script"选项配置的(默认为/etc/qemu-ifup)。一般在该脚本上创建一个TAP设备并将其中与bridge绑定起来。

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

