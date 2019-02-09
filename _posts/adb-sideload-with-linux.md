---
title: 在Linux环境下使用Sideload线刷小记
categories:
  - 笔记
  - Linux
tags:
  - Android
  - 踩坑
  - 刷机
  - Pixel Experience
date: 2019-02-09 19:25:36
thumbnail:
---

小米Mix2S上的190105版本PE发布已久，一直没有时间刷入。

<!--more-->

这次趁着刚刚过完年回来，一次性清空机子刷一遍干净的。下载好了最新版本的PE，同时把刷机脚本改了（参照本博客那篇刷机错误7解决方案笔记），以防万一。而且下载了最新开发版的FW（底包）。可以在[XiaomiFirmwareUpdater@GitHub By yshalsager](https://github.com/XiaomiFirmwareUpdater/mi-firmware-updater) & [底包下载源](https://basketbuild.com/devs/yshalsager/Xiaomi-Firmware)找到相关小米底包。

本来是想直接下载到手机里面卡刷的，然而清系统的时候一个不留神把分区全清了，这下刷机包也没了。想着进入TWRP挂载DATA分区，尝试把刷机包拉入。也没抱太大希望，在Linux下可能电脑这边挂载就已经是一件非常困难的事情，更别说拉文件进去了，当初在Win下MTP复制文件都死活拉不进去。

试着挂载DATA分区启用MTP，发现电脑成功挂载了手机的MTP，但是当拖入刷机包的时候，又和Win上一样，卡住了。（每次都想吐槽这个尴尬的传输协议，和SMB一样的无能）进度条跑了不知道有没有1/20就不再前进了，突发奇想TWRP的高级设置里面不是有ADB线刷吗，于是尝试着将手机进入了ADB Sideload线刷模式。电脑这边是Manjaro Linux已经安装了ADB TOOLS。但是使用`adb devices`命令无法看到任何设备。于是我直接重启了ADB，并且使用了SUDO。

```
sudo ./adb kill-server
sudo ./adb start-server
sudo ./adb devices
```

这时候有响应了。（突然感慨Linux上ADB比Win上方便多了，不用满世界找驱动

```
[sudo] weiyuan 的密码：
List of devices attached
5ccXXXb4	sideload
```
找到我们的刷机包目录，然后使用`adb sideload <包文件名>`进行刷入。
跑完传输的百分数就OK了。

```
adb sideload fw_polaris_miui_MIMIX2S_8.12.13_67e0e990e4_9.0.zip 
Total xfer: 1.02x           
```

按照这样依次将底包（framework）和系统包刷入即可。然后在高级选项里面把ROOT一起打上，方便后期的绿守安装，然后可以愉快的进入系统玩耍了。

![](https://ws1.sinaimg.cn/large/007i8nDUgy1g00htkod7wj31w02iox6p.jpg)

![](https://ws1.sinaimg.cn/large/007i8nDUgy1g00httwzj5j31w02iohdu.jpg)