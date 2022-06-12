---
title: Ubuntu（一）国内镜像源
tags: Ubuntu
---

平常我们使用Ubuntu自带的默认软件源。在更新链接相关的服务器的时候，可能出现无法连接的情况。导致系统的更新或者软件的安装无法正常完成。其实国内有很多的Ubuntu的镜像源。比较知名的有阿里、网易上的Ubuntu数据源。在很多大学的教育网中也包含有Ubuntu的数据源。比如：清华、中科。

# Ubuntu x86_64或者amd64

## Ubuntu-22.04

```shell
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
```

## Ubuntu-20.04

```shell
deb http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
#deb-src http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
#deb-src http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
#deb-src http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
#deb-src http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
#deb-src http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
```

## Ubuntu-18.04

```shell
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
#deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
#deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
#deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
#deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
#deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
```

## Ubuntu-16.04

```shell
deb http://mirrors.aliyun.com/ubuntu/ xenial main
#deb-src http://mirrors.aliyun.com/ubuntu/ xenial main
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main
#deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main
deb http://mirrors.aliyun.com/ubuntu/ xenial universe
#deb-src http://mirrors.aliyun.com/ubuntu/ xenial universe
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates universe
#deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates universe
deb http://mirrors.aliyun.com/ubuntu/ xenial-security main
#deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main
deb http://mirrors.aliyun.com/ubuntu/ xenial-security universe
#deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security universe
```

# Ubuntu arm或者arm64

## Ubuntu-20.04-arm64

```shell
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ focal main restricted universe multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ focal-updates main restricted universe multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ focal-backports main restricted universe multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ focal-security main restricted universe multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ focal-proposed main restricted universe multiverse
```

## Ubuntu-16.04

```shell
deb https://mirrors.aliyun.com/ubuntu-ports xenial main restricted universe multiverse
deb https://mirrors.aliyun.com/ubuntu-ports xenial-security main restricted universe multiverse
deb https://mirrors.aliyun.com/ubuntu-ports xenial-updates main restricted universe multiverse
deb https://mirrors.aliyun.com/ubuntu-ports xenial-proposed main restricted universe multiverse
deb https://mirrors.aliyun.com/ubuntu-ports xenial-backports main restricted universe multiverse
```


