---
title: Linux（二）QEMU 模拟运行 x86_64 Linux 系统
tags: Linux Qemu
---

编译内核，制作文件系统，使用Qemu模拟一个单核或者多核的x86_64架构的系统，根文件系统用ext4的形式，跟Host之间采用NFS的方式实现文件共享。

**环境准备**

```shell
sudo apt install qemu qemu-system
```

**编译内核**

```shell
make help
make x86_64_defconfig
make munuconfig
make
```

**启动内核**

```shell
qemu-system-x86_64 \
	-m 512M \ # memary size
	-smp 4 \ # cpu number
	-kernel linux/arch/x86/boot/bzImage
```

**制作磁盘镜像**

制作

```shell
qemu-img create -f raw disk.raw 512M
mkfs -t ext4 disk.raw
```

挂载

```shell
sudo mount -o loop disk.raw mnt
```

安装内核模块

```shell
sudo make modules_install INSTALL_MOD_PATH=mnt # 指定安装路径
```

启动

```shell
qemu-system-x86-64 \
	-m 512M \
	-smp 4 \
	-kernel linux/arch/x86/boot/bzImage \
	-drive format=raw,file=disk.raw \ # 指定文件作为磁盘
	-append "root=/dev/sda rw" # 内核启动参数，指定根文件系统所在设备
```

带网络启动

```shell
qemu-system-x86-64 \
	-m 512M \
	-smp 4 \
	-kernel linux/arch/x86/boot/bzImage \
	-drive format=raw,file=disk.raw \
	-append "root=/dev/sda rw" \
	-net nic -net tap,ifname=tap0  # tap dev: tap0
```

自动获取IP，安装dhcpc

```shell
sudo apt install dhcpcd
sudo systemctl enable dhcpcd
```

