---
title: docker 笔记
categories:
  - 笔记
  - Linux
tags:
  - docker
  - 容器
  - 笔记
date: 2020-10-21 16:10:00
---

本笔记记录docker使用方面的事项

持续更新

## 命令

### 端口映射

使用`-p port_host:port_container`即可

## 常用镜像

### portainer.io

### tomcat

学校刚开始教Tomcat，夭寿

该镜像位于官方仓库 <https://hub.docker.com/_/tomcat>，如果有特殊版本需求，请访问仓库页面查找对应的jdk与tomcat版本组合。 ~你看看官方那版本多的，为了各种版本组合，操碎了心，0202年了Spring Boot他不香么~

```text
CATALINA_BASE:   /usr/local/tomcat
CATALINA_HOME:   /usr/local/tomcat
CATALINA_TMPDIR: /usr/local/tomcat/temp
JRE_HOME:        /usr
CLASSPATH:       /usr/local/tomcat/bin/bootstrap.jar:/usr/local/tomcat/bin/tomcat-juli.jar
```

最简启动

```text
docker run -d -p <host_port>:8080 -v <dir to war>:/usr/local/tomcat/webapps tomcat
```

例如我将打包好的`war`文件放置到`/Users/weiyuan/.tomcat-webapps`目录下。然后我想让服务部署在宿主机端口`8080`

```text
docker run -d -p 8080:8080 -v /Users/weiyuan/.tomcat-webapps:/usr/local/tomcat/webapps tomcat
```

**注意了**，tag一定要选对，tag一定要选对，tag一定要选对！请严格按照开发环境所编译打包时的jdk version选择。否则可能会出现只能访问静态页面无法访问动态Servlet页面的问题。

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

数据备份

首先切到你要放置备份的路径，然后执行下列

```bash
docker exec CONTAINER /usr/bin/mysqldump -u root --password=root DATABASE > backup.sql
```

```bash
cat backup.sql | docker exec -i CONTAINER /usr/bin/mysql -u root --password=root DATABASE
```

### Redis

位于官方仓库，该镜像的docker hub页面 <https://hub.docker.com/_/redis/>

最简部署：

```bash
docker run -itd --name redis-test -p 6379:6379 redis
```

### ipfs

该镜像由ipfs团队构建，镜像tag为`ipfs/go-ipfs`，即是ipfs的go实现客户端。有关ipfs的内容感觉可以再开一篇了。

该镜像的docker hub页面 <https://hub.docker.com/r/ipfs/go-ipfs>

关于ipfs，简单的来理解的话，就是由个人部署众多节点位于网络上，节点之间通过P2P连接，当使用浏览器访问一个QMhash的时候，ipfs会向公网请求该数据，就像磁力链的BT一样，然后下载到本地节点，顺便传一份给浏览器。更进一步不准确的说，就是一个P2P下载器，配合一个下载面板。每个人都能搭建自己的节点，通过任意节点，都可以轻松的访问所有ipfs资源（前提是连得上“做种者”资源），得益于多节点的部署，以及ipfs协议的规范，可能将会是一次技术革新，使得分布式文件部署更加容易。

官方部署

```bash
docker run -d --name ipfs_host -e IPFS_PROFILE=server -v $ipfs_staging:/export -v $ipfs_data:/data/ipfs -p 4001:4001 -p 127.0.0.1:8080:8080 -p 127.0.0.1:5001:5001 ipfs/go-ipfs:latest
```

示例:

最简启动示范，咱们不要文件映射，太乱了，又不是不能从面板中下载，而且还可以使用docker cli将文件弄出来。需要注意的是`8080`端口为常用web端口，注意判断是否会端口重复。

```bash
docker run -d --name ipfs_host -e IPFS_PROFILE=server -p 4001:4001 -p 127.0.0.1:8080:8080 -p 127.0.0.1:5001:5001 ipfs/go-ipfs:latest
```

---
**参考资料：**

[spalladino/mysql-docker.sh at github](https://gist.github.com/spalladino/6d981f7b33f6e0afe6bb)
