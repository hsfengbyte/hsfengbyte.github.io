---
layout: post
title: 搭建NFS服务
---

**安装NFS软件包**

```shell
sudo apt install nfs-kernel-server
sudo apt install nfs-common
```

**添加NFS共享目录**

```shell
# sudo vim /etc/exports

/path *(rw,sync,no_root_squash)

# sudo chmod -R 777 /path
```

**测试NFS服务器**

```shell
sudo mount -t nfs 127.0.0.1:/path /mnt -o nolock
```

