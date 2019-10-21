---
title: docker 笔记
categories:
  - 笔记
  - Linux
tags:
  - docker
  - 容器
  - 笔记
date: 2019-10-21 16:10:00
thumbnail:
---

## 常用镜像

### portainer.io

### Mysql

该镜像由Mysql官方构建，位于docker hub官方镜像库中，直接使用mysql即可拉取。

该镜像的docker hub页面 <https://hub.docker.com/_/mysql>

name参数设定container名（可缺省），e参数设定容器启动环境变量: MYSQL_ROOT_PASSWORD设定root密码

官方最简启动：

```bash
docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag
```

示例:

设定宿主机端口3306与容器3306绑定，省略container名设定，仅保留环境变量设置Mysql root密码0000，使用latest image

```bash
docker run -p 3306:3306 -e MYSQL_ROOT_PASSWORD=0000 -d mysql:latest
```
