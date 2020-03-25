---
title: 配置各种中国源合集
categories:
  - 笔记
tags:
date: 2020-03-23 10:00:00
thumbnail:
---

本篇致力于解决中国大陆地区访问各种软件镜像源不畅的问题。相信你也遇到过，~~天天遇到~~拉源两个钟，编译5秒种的情况。

当然在条件允许的情况下，建议直接使用**路由器透明代理解决一切问题**，免配置，而且不容易出问题。

<!--more-->

## Mac OS

Mac os 中推荐使用brew作为包管理器，不推荐更换源，感觉镜像源尚未成熟。推荐使用代理解决。

启动本地代理并检查本地可以访问后，在使用brew之前运行命令

```bash
export ALL_PROXY=socks5://127.0.0.1:port
```

## Windows

Windows方面推荐使用[chocolatey](https://chocolatey.org)作为包管理器。

choco的代理配置相对会简单很多。只需要设置好系统代理之后，choco会自己走系统代理。

手动设置

```bash
choco config set proxy <locationandport>
choco config set proxyUser <username>
choco config set proxyPassword <passwordThatGetsEncryptedInFile>
```

## npm

node package manager 方面，有两种选择，使用淘宝镜像，或者使用代理。

### npm 使用淘宝镜像cnpm

使用命令（注意，若失败，请检查是否授予管理员权限执行）

```bash
npm install -g cnpm
```

### npm 使用系统代理

使用命令

```bash
npm config set proxy http://server:port
npm config set https-proxy http://server:port
```

## Maven

### Maven 使用阿里源

需要我们修改 Maven 配置文件`Settings.xml`，[官方详细说明](https://maven.apache.org/settings.html)

这里使用阿里镜像源

经过本人使用阿里镜像源，发现在项目中使用了 Spring Boot 情况下，国内镜像源**疑似**存在**资源缺失**的问题，若出现问题，建议直接使用代理解决问题

修改maven配置文件中的mirros部分

```xml
<mirrors>
    <!-- 阿里云仓库 -->
    <mirror>
        <id>alimaven</id>
        <mirrorOf>central</mirrorOf>
        <name>aliyun maven</name>
        <url>http://maven.aliyun.com/nexus/content/repositories/central/</url>
    </mirror>
</mirrors>
```

### Maven 使用本地代理

修改maven配置文件中的proxy部分

```xml
<proxies>
    <proxy>
        <id>myproxy</id>
        <active>true</active>
        <protocol>http</protocol>
        <host>proxy.somewhere.com</host>
        <port>8080</port>
        <username>proxyuser</username>
        <password>somepassword</password>
        <nonProxyHosts>*.google.com|ibiblio.org</nonProxyHosts>
    </proxy>
</proxies>
```

## apt

apt包管理器一般用于`ubuntu`以及`debian`系。

修改`/etc/apt/sources.list`文件以配置镜像源

参考使用华为云的脚本，流处理`sources.list`中的源地址

下为debian

```bash
sed -i "s@http://ftp.debian.org@https://mirrors.huaweicloud.com@g" /etc/apt/sources.list
sed -i "s@http://security.debian.org@https://mirrors.huaweicloud.com@g" /etc/apt/sources.list 
```

下为ubuntu

```bash
sed -i "s@http://.*archive.ubuntu.com@http://mirrors.huaweicloud.com@g" /etc/apt/sources.list
sed -i "s@http://.*security.ubuntu.com@http://mirrors.huaweicloud.com@g" /etc/apt/sources.list 
```

## 开源镜像站

[华为源](https://mirrors.huaweicloud.com/)
