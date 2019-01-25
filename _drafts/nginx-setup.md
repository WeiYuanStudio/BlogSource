---
title: Nginx 简单配置
date: 2019-01-25 10:14:37
thumbnail:
categories:
tags:
---
> Nginx（发音同engine x）是异步框架的 Web服务器，也可以用作反向代理，负载平衡器 和 HTTP缓存。该软件由 Igor Sysoev 创建，并于2004年首次公开发布。同名公司成立于2011年，以提供支持。

> Nginx是免费的开源软件，根据类BSD许可证的条款发布。一大部分Web服务器使用Nginx，通常作为负载均衡器。

[Nginx man page](https://linux.die.net/man/8/nginx)

## 安装Nginx服务

这里以CentOS为例`yum install nginx`。各个系统都有各种包管理，就不依次介绍了。

## 配置Nginx的配置文件

如果找不到Nginx配置文件，可以运行`nginx -t`
若安装成功，会返回
```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```
即可在这里看到配置文件的路径`/etc/nginx/nginx.conf`。接下来就可以使用vim编辑器编辑Nginx配置文件。PS:默认HTTP服务目录为`/usr/share/nginx/html`

## 启动Nginx

执行命令`nginx`即可直接启动Nginx，正常情况下，不会有任何返回。（古人云，*nix的美学，没有消息就是最好的消息。）这时候你应该可以正常访问你的站点了。

如果返回
```
nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
nginx: [emerg] still could not bind()
```
大概是因为Nginx已经运行在后台了，可以使用htop查看一下后台进程。在htop里面输入`/nginx`即可查找nginx进程了。