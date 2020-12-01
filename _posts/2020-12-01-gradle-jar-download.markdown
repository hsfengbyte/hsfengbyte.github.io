---
layout: post
titile: Gradle下载慢解决办法
---

**插件下载慢或无法下载**

Gradle 也可以用下面的方式声明使用的插件：

```
// build.gradle
plugins {
    id 'com.example.plugin', version '1.0'
}
```

其实是从 Gradle 官方的插件仓库 https://plugins.gradle.org/m2/ 下载的。

但是，众所周知的原因，某些地区会连不上，导致下载不到需要的插件，又或者插件是不对外的，存在某个私有仓库的，该如何修改或者添加额外的私有仓库地址呢？

直接修改根目录(root project)的`settings.gradle`来添加其它仓库：

```
// settings.gradle
pluginManagement {
    repositories {
        maven {
            url 'http://examle.com/maven-repo'
        }
        gradlePluginPortal()
    }
}
```

比如使用[阿里云提供的镜像](https://maven.aliyun.com/mvn/view)：

```
maven {
    url 'https://maven.aliyun.com/repository/gradle-plugin'
}
```