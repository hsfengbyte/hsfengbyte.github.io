---
layout: post
title: 编译aarch64 linux系统
---

###### 安装软件

```shell
sudo apt install qemu qemu-system
sudo apt install vim libncurses5-dev gcc make git exuberant-ctags libssl-dev bison flex libelf-dev bc
```

###### 内核编译

内核下载：

<https://www.kernel.org>

交叉编译器下载：

<https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-a/downloads>

```shell
# update .bashrc
source .bashrc

# linux compile
make ARCH=arm64 CROSS_COMPILE=aarch64-none-linux-gnu- defconfig
make ARCH=arm64 CROSS_COMPILE=aarch64-none-linux-gnu- menuconfig
make ARCH=arm64 CROSS_COMPILE=aarch64-none-linux-gnu-
```

###### QEMU启动

run with nographic:

```shell
sudo qemu-system-aarch64 \
	-M virt \
	-cpu cortex-a53 \
	-m 1024M \
	-kernel path/arch/arm64/boot/zImage \
	-nographic \
	-append "root=/dev/mmcblk0 console=ttyAMA0" \
	-sd path/ubuntu-20.04-rootfs.ext4
```

run with graphic:

```shell
sudo qemu-system-arm \
	-M virt \
	-cpu cortex-a53 \
	-m 1024M \
	-kernel path/arch/arm64/boot/zImage \
	-append "root=/dev/mmcblk0 rw console=tty0" \
	-sd path/ubuntu-20.04-rootfs.ext4
```

run with net:

```shell
sudo qemu-system-arm \
	-M virt \
	-cpu cortex-a53 \
	-m 1024M \
	-kernel path/arch/arm64/boot/zImage \
	-append "root=/dev/mmcblk0 rw console=tty0" \
	-sd path/ubuntu-20.04-rootfs.ext4 \
	-net nic -net tap,ifname=tap0
```

`备注`：qemu -dts xxx.dtb

###### 网络设置

添加br0和tap0:

```shell
# tunctl install
sudo apt install uml-utilities
# tunctl usage:
# Create: tunctl [-b] [-u owner] [-g group] [-t device-name] [-f tun-clone-device]
# Delete: tunctl -d device-name [-f tun-clone-device]

# brctl intsll
sudo apt install bridge-utils
# To manually create a bridge interface br0:
Usage: brctl [commands]
commands:
        addbr           <bridge>                add bridge
        delbr           <bridge>                delete bridge
        addif           <bridge> <device>       add interface to bridge
        delif           <bridge> <device>       delete interface from bridge
        hairpin         <bridge> <port> {on|off}        turn hairpin on/off
        setageing       <bridge> <time>         set ageing time
        setbridgeprio   <bridge> <prio>         set bridge priority
        setfd           <bridge> <time>         set bridge forward delay
        sethello        <bridge> <time>         set hello time
        setmaxage       <bridge> <time>         set max message age
        setpathcost     <bridge> <port> <cost>  set path cost
        setportprio     <bridge> <port> <prio>  set port priority
        show            [ <bridge> ]            show a list of bridges
        showmacs        <bridge>                show a list of mac addrs
        showstp         <bridge>                show bridge stp info
        stp             <bridge> {on|off}       turn stp on/off


# To add interface tap0 and tap1 to the bridge br0
brctl addif br0 tap0
brctl addif br0 tap1

# Note that you need to set the link status to up on the added interfaces.
ip link set dev tap0 up
ip link set dev tap1 up

```

网络启动、停止：


```shell
# qemu-ifup and qemu-ifdown
# create files /etc/qemu-ifup and /etc/qemu-ifdown, for example:
# /etc/qemu-ifup:

#! /bin/sh
switch=br0
ifconfig $1 up
 #ip link set $1 up
brctl addif ${switch} $1

# /etc/qemu-ifdown:

#! /bin/sh
switch=br0
brctl delif ${switch} $1
ifconfig $1 down
#ip link set $1 down
#tunctl -d $1

```

桥接网络使用：

```shell
#! /bin/bash
tunctl -t tap0 -u username
brctl addbr br0

ip link set dev tap0 up
ip link set dev br0 up

ip addr add 192.168.0.11/24 dev tap0
ip addr add 192.168.0.1/24 dev br0

#brctl addif br0 tap0
```

/etc/qemu-ifup

```shell
#! /bin/sh
# Script to bring a network (tap) device for qemu up.
# The idea is to add the tap device to the same bridge
# as we have default routing to.

# in order to be able to find brctl
PATH=$PATH:/sbin:/usr/sbin
ip=$(which ip)

if [ -n "$ip" ]; then
   ip link set "$1" up
else
   brctl=$(which brctl)
   if [ ! "$ip" -o ! "$brctl" ]; then
     echo "W: $0: not doing any bridge processing: neither ip nor brctl utility not found" >&2
     exit 0
   fi
   ifconfig "$1" 0.0.0.0 up
fi

switch=$(ip route ls | \
    awk '/^default / {
          for(i=0;i<NF;i++) { if ($i == "dev") { print $(i+1); next; } }
         }'
        )

# only add the interface to default-route bridge if we
# have such interface (with default route) and if that
# interface is actually a bridge.
# It is possible to have several default routes too
for br in $switch; do
    if [ -d /sys/class/net/$br/bridge/. ]; then
        if [ -n "$ip" ]; then
          ip link set "$1" master "$br"
        else
          brctl addif $br "$1"
        fi
        exit    # exit with status of the previous command
    fi
done

echo "W: $0: no bridge for guest interface found" >&2
```

