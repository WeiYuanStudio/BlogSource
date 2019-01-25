---
title: vsftpd 简单配置
date: 2019-01-25 11:04:19
thumbnail:
categories:
tags:
---

## 普通的FTP无法登入
默认情况下，直接使用普通的FTP是无法连接到FTP服务器的(例如在文件管理器输入ftp://<ip>:80)，需要使用SFTP连接。(例如在文件管理器输入sftp://<ip>:80),需要修改`/etc/sysconfig/selinux`改为`selinux=disabled`，并重启服务。