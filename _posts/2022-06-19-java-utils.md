---
title: Java学习笔记（七）工具类库
tags: Java
---

使用这些工具类库提高我们的开发效率，减少无关紧要的代码，下面给大家简单分享一下，特别适合新手开发和使用。

<!--more-->

# apache commons 工具类库

## commons-lang3

Maven 依赖

```xml

<dependency>
  <groupId>org.apache.commons</groupId>
  <artifactId>commons-lang3</artifactId>
  <version>3.12.0</version>
</dependency>
```

## commons-collections

Maven 依赖

```xml

<dependency>
  <groupId>org.apache.commons</groupId>
  <artifactId>commons-collections4</artifactId>
  <version>4.4</version>
</dependency>
```

## commons-beanutils

Maven 依赖

```xml

<dependency>
  <groupId>commons-beanutils</groupId>
  <artifactId>commons-beanutils</artifactId>
  <version>1.9.4</version>
</dependency>
```

## commons-io

Maven 依赖

```xml

<dependency>
  <groupId>commons-io</groupId>
  <artifactId>commons-io</artifactId>
  <version>2.8.0</version>
</dependency>
```

# Google guava 工具类库

Maven 依赖

```xml
<dependency>
  <groupId>com.google.guava</groupId>
  <artifactId>guava</artifactId>
  <version>30.1.1-jre</version>
</dependency>
```

# Hutool 工具类库

官网地址： [https://www.hutool.cn/](https://www.hutool.cn/)

# Json 处理工具

- org.json

- net.sf.json

- json-simple

- gson：最强大，对象，集合嵌套对象，对象与json之间转换毫无压力。

- jackson：功能最全，不是最快的，但是相对稳定，功能齐全。

- fastjson：问题最多，反序列化，解析还会有很多问题，只一味追求快。git上看看就呵呵了，issue1.5K！还在用说明你心真大。
