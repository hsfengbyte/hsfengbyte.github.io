---
title: Nginx 入门指南
tags: Nginx
---

# Nginx是什么？

> “Nginx 是一款轻量级的 HTTP 服务器，采用事件驱动的异步非阻塞处理方式框架，这让其具有极好的 IO 性能，时常用于服务端的反向代理和负载均衡。”

<!--more-->

这是大多数开发者对 Nginx 的定义。

（什么？听不懂，反向代理、负载均衡这都什么鬼？那么请稍安勿躁，请君带着疑问往下看）

Nginx 是一款 http 服务器 （或叫web服务器）。它是由俄罗斯人 伊戈尔·赛索耶夫为俄罗斯访问量第二的 Rambler.ru 站点开发的，并于2004年首次公开发布的。

> web服务器：负责处理和响应用户请求，一般也称为http服务器，如 Apache、IIS、Nginx
>
> 应用服务器：存放和运行系统程序的服务器，负责处理程序中的业务逻辑，如 Tomcat、Weblogic、Jboss（现在大多数应用服务器也包含了web服务器的功能）

Nginx 是什么，总结一下就是这些：

- 一种轻量级的web服务器

- 设计思想是事件驱动的异步非阻塞处理（类node.js）

- 占用内存少、启动速度快、并发能力强

- 使用C语言开发

- 扩展性好，第三方插件非常多

- 在互联网项目中广泛应用

# 为什么要学习Nginx？

Nginx 是全球排名前三的服务器，并且近年来用户增长非常快。

有人统计，世界上约有三分之一的网址采用了Nginx。在大型网站的架构中，Nginx被普遍使用，如 百度、阿里、腾讯、京东、网易、新浪、大疆等。

Nginx 安装简单，配置简洁，作用却无可替代。Nginx 是运维和后端的必修课，也是前端进阶的必修课。

因为掌握了Nginx，能让前端站得更高，更好的设计系统架构，更好的选择问题的解决方案，更好的服务业务开发。
所以综上所述，Nginx不得不学，前端也一样不能落后。

# 如何学习Nginx？

## 安装及相关组件

openssl安装

```shell
[root@localhost src]# tar zxvf openssl-fips-2.0.10.tar.gz
省略安装内容...
[root@localhost src]# cd openssl-fips-2.0.10
[root@localhost openssl-fips-2.0.10]# ./config && make && make install
省略安装内容...
```

pcre安装

```shell
[root@localhost src]# tar zxvf pcre-8.40.tar.gz
省略安装内容...
[root@localhost src]# cd pcre-8.40
[root@localhost pcre-8.40]# ./configure && make && make install
省略安装内容...
```

zlib安装

```shell
[root@localhost src]# tar zxvf zlib-1.2.11.tar.gz
省略安装内容...
[root@localhost src]# cd zlib-1.2.11
[root@localhost zlib-1.2.11]# ./configure && make && make install
省略安装内容...
```

nginx安装

```shell
[root@localhost src]# tar zxvf nginx-1.10.2.tar.gz
省略安装内容...
[root@localhost src]# cd nginx-1.10.2
[root@localhost nginx-1.10.2]# ./configure && make && make install
省略安装内容...
```

## 启动和重启

以下是启动、停止、重启或重新加载 Nginx 的一些基本操作命令的列表。

### 启动

可以从命令行启动 Nginx：

```shell
$ sudo systemctl start nginx
```

此命令不产生任何输出。

如果您使用的是没有 systemd 的 Linux 发行版(低于 Redhat 7 的版本)，那么要启动 Nginx，请键入以下命令：

```shell
$ sudo service start nginx
```

或者从源码编译安装的 nginx 启动命令：

```shell
$ sudo nginx -s start
```

或者对于较旧的 Ubuntu Linux 版本：

```shell
sudo /etc/init.d/nginx start
```

### 启用服务

上述命令仅本次启动 Nginx 有效。每次重新启动系统后需要再次手动启动 Nginx。

建议在系统启动时自动启动 Nginx 服务，而不是手动启动 Nginx 服务。要启用 Nginx 服务，请使用以下命令：

```shell
$ sudo systemctl enable nginx
```

或者，如果您使用的是没有 systemd 的 Linux 发行版，则使用以下命令：

```shell
$ sudo service nginx enable
```

### 停止

即使有打开的连接，停止 Nginx 也会直接关闭所有 Nginx 工作进程。我们可以通过两种方式停止 Nginx 进程。

第一种停止 Nginx 方法是使用 stop 命令调用 Nginx。使用以下命令之一停止 Nginx：

```shell
$ sudo systemctl stop nginx
```

或者，如果您使用的是没有 systemd 的 Linux 发行版，则使用以下命令：

```shell
$ sudo service stop nginx
```

或者从源码编译安装的 nginx 停止命令：

```shell
$ sudo nginx -s stop
```

或者对于较旧的 Ubuntu Linux 版本：

```shell
$ sudo /etc/init.d/nginx stop
```

第二种停止 Nginx 的方法是向 Nginx 主进程发送信号。默认情况下，Nginx 将其主进程 ID 存储到 /var/run/nginx.pid。

这是向 Nginx 主进程发送 QUIT（正常关机）信号的命令：

```shell
$ kill -QUIT $( cat /var/run/nginx.pid )
```

### 重启

重启选项是停止然后启动 Nginx 服务器的快速操作。

使用以下命令之一重新启动 Nginx：

```shell
$ sudo systemctl restart nginx
```

或者，如果您使用的是没有 systemd 的 Linux 发行版，则使用以下命令：

```shell
$ sudo service restart nginx
```

或者对于较旧的 Ubuntu Linux 版本：

```shell
sudo /etc/init.d/nginx restart
```

### 重新加载

每当我们更改其配置时，我们都需要重新启动或重新加载 Nginx。

重新加载选项将加载新配置，使用新配置启动新的工作进程并优雅地关闭旧的工作进程。

要重新加载 Nginx，请键入以下命令之一：

```shell
$ sudo systemctl reload nginx
```

或者，如果您使用的是没有 systemd 的 Linux 发行版，则使用以下命令：

```shell
$ sudo service reload nginx
```

或者从源码编译安装的 nginx 重载命令：

```shell
$ sudo nginx -s reload
```

### 测试配置

每当我们对 Nginx 服务器的配置文件进行更改或编辑时，最好在重新启动或重新加载服务之前测试配置。

使用以下命令测试 Nginx 配置是否存在任何语法或系统错误：

```shell
$ sudo nginx -t
```

两者的输出将如下所示：

```shell
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

如果有任何错误，这些命令将打印详细消息。

## 修改配置

配置文件

```shell
[root@localhost ~]# vim /usr/local/nginx/conf/nginx.conf
```

nginx 默认配置文件简介：

```
# 首尾配置暂时忽略
server {
        # 当nginx接到请求后，会匹配其配置中的service模块
        # 匹配方法就是将请求携带的host和port去跟配置中的server_name和listen相匹配
        listen       8080;
        server_name  localhost; # 定义当前虚拟主机（站点）匹配请求的主机名

        location / {
            root   html; # Nginx默认值
            # 设定Nginx服务器返回的文档名
            index  index.html index.htm; # 先找根目录下的index.html，如果没有再找index.htm
        }
}
# 首尾配置暂时忽略
```

server{ } 其实是包含在 http{ } 内部的。每一个 server{ } 是一个虚拟主机（站点）。

上面代码块的意思是：当一个请求叫做 `localhost:8080` 请求nginx服务器时，该请求就会被匹配进该代码块的 server{ } 中执行。

当然 nginx 的配置非常多，用的时候可以根据文档进行配置。

> 英文文档：[nginx.org/en/docs/](http://nginx.org/en/docs/)
>
> 中文文档：[www.nginx.cn/doc/](https://www.nginx.cn/doc/)

# Nginx有哪些应用？

主要有4大应用。

## 动静分离

动静分离其实就是 Nginx 服务器将接收到的请求分为动态请求和静态请求。

静态请求直接从 nginx 服务器所设定的根目录路径去取对应的资源，动态请求转发给真实的后台（前面所说的应用服务器，如图中的Tomcat）去处理。

这样做不仅能给应用服务器减轻压力，将后台api接口服务化，还能将前后端代码分开并行开发和部署。

（传送门：[nginx动静分离的好处](https://www.php.cn/nginx/424631.html)）

```
server {
        listen       8080;
        server_name  localhost;

        location / {
            root   html; # Nginx默认值
            index  index.html index.htm;
        }

        # 静态化配置，所有静态请求都转发给 nginx 处理，存放目录为 my-project
        location ~ .*\.(html|htm|gif|jpg|jpeg|bmp|png|ico|js|css)$ {
            root /usr/local/var/www/my-project; # 静态请求所代理到的根目录
        }

        # 动态请求匹配到path为'node'的就转发到8002端口处理
        location /node/ {
            proxy_pass http://localhost:8002; # 充当服务代理
        }
}
```

访问静态资源 nginx 服务器会返回 my-project 里面的文件，如获取 index.html。

## 反向代理

### 反向代理是什么？

反向代理其实就类似你去找代购帮你买东西（浏览器或其他终端向nginx请求），你不用管他去哪里买，只要他帮你买到你想要的东西就行（浏览器或其他终端最终拿到了他想要的内容，但是具体从哪儿拿到的这个过程它并不知道）。

### 反向代理的作用

1. 保障应用服务器的安全（增加一层代理，可以屏蔽危险攻击，更方便的控制权限）

2. 实现负载均衡（稍等~下面会讲）

3. 实现跨域（号称是最简单的跨域方式）

### 配置反向代理

配置一个简单的反向代理是很容易的，代码如下：

```
server {
        listen       8080;
        server_name  localhost;

        location / {
            root   html; # Nginx默认值
            index  index.html index.htm;
        }

        proxy_pass http://localhost:8000; # 反向代理配置，请求会被转发到8000端口
}
```

反向代理的表现很简单。那上面的代码块来说，其实就是向nginx请求 `localhost:8080` 跟请求 `http://localhost:8000` 是一样的效果。（跟代购的原理一样）

这是一个反向代理最简单的模型，只是为了说明反向代理的配置。但是现实中反向代理多数是用在负载均衡中。

反向代理应用十分广泛，CDN 服务就是反向代理经典的应用场景之一。除此之外，反向代理也是实现负载均衡的基础，很多大公司的架构都应用到了反向代理。

## 负载均衡

### 负载均衡是什么？

随着业务的不断增长和用户的不断增多，一台服务已经满足不了系统要求了。这个时候就出现了服务器集群。

在服务器集群中，Nginx 可以将接收到的客户端请求“均匀地”（严格讲并不一定均匀，可以通过设置权重）分配到这个集群中所有的服务器上。这个就叫做负载均衡。

### 负载均衡的作用

- 分摊服务器集群压力
- 保证客户端访问的稳定性

前面也提到了，负载均衡可以解决分摊服务器集群压力的问题。除此之外，Nginx还带有健康检查（服务器心跳检查）功能，会定期轮询向集群里的所有服务器发送健康检查请求，来检查集群中是否有服务器处于异常状态。

一旦发现某台服务器异常，那么在这以后代理进来的客户端请求都不会被发送到该服务器上（直健康检查发现该服务器已恢复正常），从而保证客户端访问的稳定性。

### 配置负载均衡

配置一个简单的负载均衡并不复杂，代码如下：

```
# 负载均衡：设置domain
upstream domain {
    server localhost:8000;
    server localhost:8001;
}
server {
        listen       8080;
        server_name  localhost;

        location / {
            # root   html; # Nginx默认值
            # index  index.html index.htm;

            proxy_pass http://domain; # 负载均衡配置，请求会被平均分配到8000和8001端口
            proxy_set_header Host $host:$server_port;
        }
}
```

负载均衡成功后可以看到访问 localhost:8080 有时会访问到8000端口的页面，有时会访问到8001端口的页面。

受集群单台服务器内存等资源的限制，负载均衡集群的服务器也不能无限增多。但因其良好的容错机制，负载均衡成为了实现高可用架构中必不可少的一环。

## 正向代理

正向代理跟反向道理正好相反。拿上文中的那个代购例子来讲，多个人找代购购买同一个商品，代购找到买这个的店后一次性给买了。这个过程中，该店主是不知道代购是帮别代买买东西的。那么代购对于多个想买商品的顾客来讲，他就充当了正向代理。

正向代理，意思是一个位于客户端和原始服务器(origin server)之间的服务器，为了从原始服务器取得内容，客户端向代理发送一个请求并指定目标(原始服务器)，然后代理向原始服务器转交请求并将获得的内容返回给客户端。客户端才能使用正向代理。当你需要把你的服务器作为代理服务器的时候，可以用Nginx来实现正向代理。

科学上网（俗称翻墙）其实就是一个正向代理工具。

该科学上网会将想访问墙外服务器 server 的网页请求，代理到一个可以访问该网站的代理服务器 proxy 上。这个 proxy 把墙外服务器 server 上获取的网页内容，再转发给客户。

代理服务器 proxy 就是 Nginx 搭建的。
