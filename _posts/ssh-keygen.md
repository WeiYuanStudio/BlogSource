---
title: SSH配置密钥对实现免密登陆
thumbnail: https://ws2.sinaimg.cn/large/007i8nDUgy1fxz4s19f9pj31hc0u0guc.jpg
date: 2018-10-21 9:26:00
categories:
 - 笔记
 - 网络
tags:
 - SSH
 - 安全
---

先引用Arch Wiki 上的一段话。对SSH Key 做简短的说明

>SSH 密钥对总是成双出现的，一把公钥，一把私钥。公钥可以自由的放在您所需要连接的 SSH 服务器上，而私钥必须稳妥的保管好。
所谓"公钥登录"，原理很简单，就是用户将自己的公钥储存在远程主机上。登录的时候，远程主机会向用户发送一段随机字符串，用户用自己的私钥加密后，再发回来。远程主机用事先储存的公钥进行解密，如果成功，就证明用户是可信的，直接允许登录 shell，不再要求密码。这样子，我们即可保证了整个登录过程的安全，也不会受到中间人攻击。
<!--more-->
假设你需要登陆的主机为A，本地主机为B。那么需要 **在B上生成密钥对** 。且将 **B上生成的公钥拷贝到A上**

## 在B主机上生成密钥
执行命令`ssh-keygen`将会随机生成一对密钥对（即一个公钥，一个私钥，需要拷贝到A主机的是公钥）,接着，会提示`Enter file in which to save the key(/root/.ssh/id_rsa):`,即要求输入你需要保存SSH Key的路径。可以直接回车保存到默认路径`/root/.ssh/id_rsa`。然后又接着出现了`Enter passphrase`的提示，至于这个表示没有用过也没有测试过。改天试用一下把，到时候大概会补充 ~~懒癌发作，flag+1~~ 查阅了下[文档](https://www.ssh.com/ssh/passphrase)，大概是用于加密SSH Key 文件的密码。

>passphrase的目的通常是加密私钥。这使得密钥文件本身对攻击者无用。文件从备份或退役硬件中泄漏的情况并不少见，黑客通常会从受感染的系统中泄露文件。

## 拷贝B的公钥至目标主机A
执行`scp B上的公钥路径 将要登陆A的用户@网络地址:目标目录`

示例`scp /root/.ssh/id_rsa.pub root@192.168.1.1:/root`

## 登陆目标主机A，并将公钥拷贝至配置文件
登陆后，cd到刚才拷贝公钥的目录。例如执行`cd /root/`然后使用命令`cat id_rsa.pub >> /root/.ssh/authorized_keys`将刚才拷贝到目标主机A的公钥拷贝到`/root/.ssh/authorized_keys`文件中

## Enjoy
