---
title: Zookeeper
tags: zookeeper
---

一个典型的分布式数据一致性的解决方案，分布式应用程序可以基于它实现诸如数据发布/订阅、负载均衡、命名服务、分布式协调/通知、集群管理、Master 选举、分布式锁和分布式队列等功能。

# Zookeeper 权限控制 ACL

## ACL 命令行

- getAcl 命令：获取某个节点的 acl 权限信息。

- setAcl 命令：设置某个节点的 acl 权限信息。

- addauth 命令：输入认证授权信息，注册时输入明文密码，加密形式保存。


## ACL 构成

zookeeper 的 acl 通过 [scheme: id: permissions] 来构成权限列表。

1. scheme：代表采用的某种权限机制，包括 world、auth、digest、ip、super 几种。
2. id：代表允许访问的用户。

3. permissions：权限组合字符串，由 cdrwa 组成，其中每个字母代表支持不同权限， 创建权限 create(c)、删除权限 delete(d)、读权限 read(r)、写权限 write(w)、管理权限admin(a)。

zooKeeper的权限控制是基于每个znode节点的，需要对每个节点设置权限每个znode支持设置多种权限控制方案和多个权限子节点不会继承父节点的权限，客户端无权访问某节点，但可能可以访问它的子节点。

## 权限模式

### 授权方式

world：只有一个用户： anyone， 代表登录zokeeper所有人(默认)

ip：对客户端使用IP地址认证

auth：使用已添加认证的用户认证

digest：1·使用“用户名:密码”方式认证

### 授予的权限

world: 它下面只有一个id, 叫anyone, world:anyone代表任何人，zookeeper中对所有人有权限的结点就是属于world:anyone的

auth: 它不需要id, 只要是通过authentication的user都有权限(zookeeper支持通过kerberos来进行authencation, 也支持username/password形式的authentication)

digest: 它对应的id为username:BASE64(SHA1(password))，它需要先通过username:password形式的authentication

ip: 它对应的id为客户机的IP地址，设置的时候可以设置一个ip段，比如ip:192.168.1.0/16, 表示匹配前16个bit的IP段

super: 在这种scheme情况下，对应的id拥有超级权限，可以做任何事情(cdrwa)
