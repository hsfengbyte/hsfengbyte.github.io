---
title: Linux（五）DHCP
tags: Linux
---

简单介绍在linux系统上搭建dhcp服务实现域名解析服务。

<!--more-->

安装DHCP服务软件

```shell
sudo apt install isc-dhcp-server
```

配置DHCP服务

配置isc-dhcp-server

```shell
# /etc/default/isc-dhcp-server

# enp0s3 is host network card
INTERFACES="enp0s3"
```

配置dhcpd.conf

```shell
# /etc/dhcp/dhcpd.conf

#subnet后跟子网网段，netmask后跟子网掩码
subnet 10.0.0.0 netmask 255.255.255.0 {
  #地址池
  range 10.0.0.10 10.0.0.100;
  #DNS服务器地址(多个地址用","隔开)
  #option domain-name-servers 202.206.192.33, 223.5.5.5;
  #为所分配的域分配域名
  #option domain-name "mylab.com";
  #为所分配的主机分发子网掩码
  option subnet-mask 255.255.255.0;
  #分发默认网关
  option routers 10.0.0.1;
  #分发广播地址
  option broadcast-address 10.0.0.255;
  #默认租期时间(秒)
  default-lease-time 600;
  #最大租期时间(秒)
  max-lease-time 7200;
}
```

重启DHCP

```shell
sudo systemctl restart isc-dhcp-server
```

查看DHCP

```shell
sudo netstat -uap
```

