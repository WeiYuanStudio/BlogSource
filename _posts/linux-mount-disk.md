---
title: Linux 挂载第二块硬盘
date: 2019-01-19 13:24:55
thumbnail:
categories:
  - 笔记
  - Linux
tags:
 - Linux
 - 系统配置
---

小米笔记本的240G硬盘始终都还是不太够用。从一台旧联想上拆了块Intel SSD。不像U盘那样，装上去后系统并没有直接显示这块硬盘。这时候需要手动挂载硬盘了。由于手动挂载的机会也不多。所以记录一下。  
<!--more-->
## 先查看一下我们的硬盘信息  
运行`$ sudo fdisk -l`查看硬盘分区信息   
```bash
[sudo] weiyuan 的密码：
Disk /dev/nvme0n1：238.5 GiB，256060514304 字节，500118192 个扇区
Disk model: SAMSUNG MZVLB256HAHQ-00000              
单元：扇区 / 1 * 512 = 512 字节
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节
磁盘标签类型：dos
磁盘标识符：0x661baeed

设备           启动      起点      末尾      扇区   大小 Id 类型
/dev/nvme0n1p1           2048 464230001 464227954 221.4G 83 Linux
/dev/nvme0n1p2      464230002 500103449  35873448  17.1G 82 Linux swap / Solaris


Disk /dev/sda：335.4 GiB，360080695296 字节，703282608 个扇区
Disk model: INTEL SSDSCKKW36
单元：扇区 / 1 * 512 = 512 字节
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节
磁盘标签类型：dos
磁盘标识符：0xce24edc0

设备       启动  起点      末尾      扇区   大小 Id 类型
/dev/sda1        2048 703281151 703279104 335.4G 83 Linux

```
上面那块 SAMSUNG MZVLB256HAHQ-00000    是我笔记本的原装固态硬盘。: INTEL SSDSCKKW36是我后来加装的硬盘。在运行这行命令之前，我已经使用Manjaro自带的分区工具**GParted**分区过了，格式为**ext4**为了不出现更换硬盘接口顺序后出现挂载问题。使用UUID来识别硬盘而不是以前的旧方法
> ## 旧方法  
>一、检测硬盘能否被识别  
>`# fdisk -l`
>
>二、挂载硬盘  
>1、在本地硬盘中临时创建一个目录  
>`#mkdir /mnt/sdx`  
>2、挂载第二块硬盘中的一个分区/dev/sdx1到sdx  
>`#mount /dev/sdx1 /mnt/sdx`  
>3、查看是否被挂载  
>`# df -h`
>
>三、卸载硬盘  
>`#umount /dev/sdx1`
## 查看需要挂载的分区的UUID
运行`$ sudo blkid`
```bash
[sudo] weiyuan 的密码：
/dev/nvme0n1: PTUUID="661baeed" PTTYPE="dos"
/dev/nvme0n1p1: UUID="X-X-X-X-X" TYPE="ext4" PARTUUID="661baeed-01"
/dev/nvme0n1p2: UUID="X-X-X-X-X" TYPE="swap" PARTUUID="661baeed-02"
/dev/sda1: UUID="X-X-X-X-X" TYPE="ext4" PARTUUID="ce24edc0-01"
```
为了避免可能会存在的隐私泄漏风险。故将UUID码掉处理。
记下需要挂载分区的UUID，复制此UUID，并修改`/etc/fstab`文件。
比如我想将该分区挂载到根目录下的/disk2。可以按照如下填写。
```
UUID=XXXXXXXX /disk2 ext4 defaults 0
```
![](https://ws1.sinaimg.cn/large/007i8nDUgy1fzd1f3mlmwj30vi0ewta6.jpg)
Reboot，然后你应该能看见磁盘正常挂载了。

关于fstab配置文件,这里放一个[wiki链接](https://wiki.archlinux.org/index.php/Fstab_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)#atime_%E5%8F%82%E6%95%B0)