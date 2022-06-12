---
title: Ubuntu（二）根文件系统制作
tags: Ubuntu
---

Ubuntu根文件系统制作，以及基本开发包安装。

### 1. 必需软件

```shell
sudo apt install qemu qemu-system qemu-user-static binfmt-support debootstrap
```

### 2. 下载地址

ubuntu core: <http://cdimage.ubuntu.com/ubuntu-base/releases/>

例，下载ubuntu-base-20.04.1-base-arm64.tar.gz:

```shell
wget -c http://cdimage.ubuntu.com/ubuntu-base/releases/20.04.1/release/ubuntu-base-20.04.1-base-arm64.tar.gz
```

### 3. 安装制作

```shell
# 制作磁盘
# dd if=/dev/zero of=ubuntu-20.04-rootfs.img bs=1M count=1024
qemu-img create -f raw ubuntu.ext4 8G
# 扩展磁盘
# qemu-img resize ubuntu.ext4 +4G
# resize2fs ubuntu.ext4
# 格式化磁盘文件系统
# mkfs.ext4 -F -L ROOTFS ubuntu-20.04-rootfs.img
mkfs -t ext4 ubuntu.ext4
# 挂载磁盘
mkdir ubuntu
sudo mount ubuntu.ext4 ubuntu
# 解压制作
sudo tar -xvf ubuntu-base-20.04.1-base-arm64.tar.gz -C ubuntu
sudo cp /usr/bin/qemu-aarch64-static ubuntu/usr/bin
sudo cp /etc/resolv.conf ubuntu/etc/resolv.conf
sudo chroot ubuntu # change root
###################################
useradd -G sudo -m -s /bin/bash username
passwd username

echo "hsfengbyte" > /etc/hostname
echo "127.0.0.1 localhost.localdomain localhost" > /etc/hosts
apt update
apt upgrade
apt install gcc cmake make

# basic installation
apt install vim libncurses5-dev gcc make git exuberant-ctags libssl-dev bison flex libelf-dev bc
apt install dhcpcd5 net-tools network-manager iproute2 iproute2-doc inetutils-ping udev
sudo ssh
apt install kmod tree

exit # exit
###################################
sudo umount ubuntu
```

