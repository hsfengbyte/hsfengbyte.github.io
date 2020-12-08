---
layout: post
title: 解决Github下载速度过慢问题
---

**解决Github下载速度过慢问题**

**A、修改hosts文件**

- 打开IP查询网站：https://www.ipaddress.com/
- 分别输入网址：raw.githubusercontent.com、github.global.ssh.fastly.net、github.com。查询IP，or：

```bash
#example：
$ nslookup github.com  #查询IP
12
```

- 打开hosts文件

```bash
$ sudo vim /etc/hosts
1
```

- 写入查到的IP，保存并关闭

```bash
199.232.68.133  raw.githubusercontent.com  #vim-plug
0.0.0.0 github.global.ssh.fastly.net  #git clone
0.0.0.0 github.com  #git clone
123
```

- 刷新DNS

```bash
$ sudo apt-get install nscd  #安装依赖：nscd
$ sudo /etc/init.d/nscd restart  #刷新
```