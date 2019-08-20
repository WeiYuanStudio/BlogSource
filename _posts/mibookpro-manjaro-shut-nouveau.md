---
title: 小米笔记本Pro安装Manjaro Linux解决无法正常关机问题（显卡折腾笔记）
categories:
  - 踩坑
tags:
  - Linux
  - 系统
  - 踩坑
date: 2019-01-20 18:13:05
thumbnail:
---

~~前段时间在小米笔记本上安装Manjaro遇到了安装完后无法关机的问题。经过多次测试，找到了解决方法。~~

文章更新，用于记录有关于Manjaro上折腾显示方面问题的记录

<!--more-->

# bumblebee的使用

推荐阅读：

[Systemd 入门教程：命令篇 By ruanyf](http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html)

[systemd By Arch Wiki](https://wiki.archlinux.org/index.php/Systemd_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))

## 如果想要使用bumblebee

需要保证

 - 当前用户已经加入了bumblebee的用户组

使用`usermod -a uname -G bumblebee`可以加入bumblebee用户组

 - 保证bumblebee daemon的运行

在RHEL7开始，使用**systemd**替代**init**。链接bumblebee daemon配置文件（开启bumblebee的守护）`sudo systemctl enable bumblebeed`

## 使用独立显卡运行某个命令

在前加入`primusrun`或者`optirun`

`glxgears`可以运行一个包含三个齿轮的动画窗口,用于测试显卡性能。同时命令行不断会打印5秒内的平均帧率。

如果发现帧率基本锁死在60，可能是因为垂直同步开启了，可以尝试在`glxgears`前加入`vblank_mode=0`解锁

（与显卡配置文件中的垂直同步无关，我的情况是i卡运行时锁帧60，解锁起飞7k帧，但是屏幕播放视频，弹幕撕裂，后面靠配置文件启动TearFree解决了这个问题，BTW，独显可能在该测试中性能不如集显，不知道是针对性的优化问题还是因为闭源显驱的问题，我测试的情况和另外一个找到的资料一样，都是集显7k起飞，独显1k，通过nvidia-settings工具查看显卡运行频率和温度证实的确是独显在运行）

`optirun -b none nvidia-settings -c :8`打开nvidia显卡控制面板，需要bumblebee运行，否则报错：

```
ERROR: Unable to find display on any available system
```

## 解决i卡播放视频撕裂的问题

[I卡Arch Wiki](https://wiki.archlinux.org/index.php/Intel_graphics)

通过加入配置文件`/etc/X11/xorg.conf.d/20-intel.conf`

```
Section "Device"
   Identifier  "Intel Graphics"
   Driver      "intel"
   Option      "TearFree"  "true"
EndSection
```

解决了i卡播放视频画面撕裂的问题，整体观感改善很大，而且已通过运动相机慢动作测试，真不是玄学

测试视频可以在油管搜索4k sync test关键词找到。

# 在该笔记本上安装系统请选择non-free驱动，避免安装nouveau

**疑似nouveau驱动会导致该机无法正常关机**

多次测试，Manjaro18在小米笔记本Pro上安装的时候请注意在U盘引导安装启动的时候就应该选择**non-free**（闭源驱动）否则会出现无法关机和无法正常重启的问题。（和桌面版本无关，经过我的测试xfce和KDE桌面都是一样会遇到这个问题的）如果非要选择free驱动的话。可以在系统安装完毕后编辑`$sudo vim /etc/modprobe.d/blacklist.conf` 在里面加入一行`blacklist nouveau`屏蔽开源驱动，也可以做到正常关机或重启。（我一早晨全耗在测试上，测试了好几遍，才发现在安装的时候就应该选择non-free驱动。电脑被我无数次强制关机，心痛硬盘啊！希望同样遇到这个问题的朋友能找到我这篇文章，解决这个问题。而不是向我一样，栽在坑里半天。

命令行运行`mhwd`可以查看到当前驱动信息

下面贴一段我自己的

```
> 0000:01:00.0 (0302:10de:1d12) Display controller nVidia Corporation:
--------------------------------------------------------------------------------
                  NAME               VERSION          FREEDRIVER           TYPE
--------------------------------------------------------------------------------
video-hybrid-intel-nvidia-bumblebee            2018.08.09               false            PCI
video-hybrid-intel-nvidia-390xx-bumblebee            2018.08.09               false            PCI
          video-nvidia            2018.08.09               false            PCI
    video-nvidia-390xx            2018.08.09               false            PCI
           video-linux            2018.05.04                true            PCI


> 0000:00:02.0 (0300:8086:5917) Display controller Intel Corporation:
--------------------------------------------------------------------------------
                  NAME               VERSION          FREEDRIVER           TYPE
--------------------------------------------------------------------------------
video-hybrid-intel-nvidia-bumblebee            2018.08.09               false            PCI
video-hybrid-intel-nvidia-390xx-bumblebee            2018.08.09               false            PCI
           video-linux            2018.05.04                true            PCI
            video-vesa            2017.03.12                true            PCI

```

~~经过一段时间的使用后发现。Manjaro在小米笔记本Pro上运行，在开关机的时候都会出现一两秒的花屏。不过目前来说并没有遇到致命性问题~~(这个问题不知道在什么时候就自己修复了，可能是某次推送更新的时候更新了驱动)

[这个链接](https://wiki.manjaro.org/index.php?title=Configure_Graphics_Cards)，是Manjaro官方wiki讲解有关于显卡配置问题的。

# 最后

通过测试发现启用i卡的垂直同步后，已经完美解决了笔记本的屏幕撕裂问题。所以也就把bumblebee daemon关了。平时没有能用上独显的机会。根据本人接近一年的使用体验，小米笔记本Pro 顶配版在使用Manjaro Gnome版本时的兼容表现很令人满意。没有巨坑出现。

~~下一台笔记本买XPS或者Thinkpad纯集显本，不想再折腾了~~