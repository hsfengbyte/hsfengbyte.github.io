---
layout: post
title: Linux kernel学习笔记
---

日志级别一共有8个级别，printk的日志级别定义如下（在include/linux/kernel.h中）：

```c
#define KERN_EMERG 0
#define KERN_ALERT 1
#define KERN_CRIT 2
#define KERN_ERR 3
#define KERN_WARNING 4
#define KERN_NOTICE 5
#define KERN_INFO 6
#define KERN_DEBUG 7
```