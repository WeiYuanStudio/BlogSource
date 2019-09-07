---
title: Java Web Servlet 笔记
date: 2019-09-07 18:56:00
thumbnail:
categories:
 - 笔记
 - Java
tags:
 - Java
 - JavaEE
 - Servlet
---

# 安装环境

安装JDK以及IDE（本人选用IDEA EDU授权版本，支持JavaEE开发），服务端选择TomCat9

## Windows安装TomCat9
TomCat用的是Choco包管理器安装。安装完毕后，已经自动配置了环境环境变量。命令行`tomcat9`就可以启动了。

安装完毕后**tomcat.exe**位于choco的bin文件夹中，其他文件位于Windows隐藏的的系统盘文件夹ProgramData中

 - conf 文件夹中存放的是xml配置文件
    - server.xml **最重要**配置端口信息，站点信息等
    - tomcat-users.xml 配置tomcat面板登录用户
    - context.xml 配置xml存放路径
    - web.xml 配置servlet，定向url到具体servl class资源
 - logs 存放log
 - temp
 - webapps 目录下存放多个站点文件夹，通过`ip:8080/文件夹名`即可访问
 - work 貌似是运行时jsp编译的缓存文件夹，大概是 jsp -> java -> class?
