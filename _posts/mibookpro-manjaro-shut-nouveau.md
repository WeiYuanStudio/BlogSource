---
title: 小米笔记本Pro安装Manjaro Linux解决无法正常关机问题
categories:
  - 踩坑
tags:
  - Linux
  - 系统
  - 踩坑
date: 2019-01-20 18:13:05
thumbnail:
---

前段时间在小米笔记本上安装Manjaro遇到了安装完后无法关机的问题。经过多次测试，找到了解决方法。

<!--more-->

## 是显驱问题

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

经过一段时间的使用后发现。Manjaro在小米笔记本Pro上运行，在开关机的时候都会出现一两秒的花屏。不过目前来说并没有遇到致命性问题

[这个链接](https://wiki.manjaro.org/index.php?title=Configure_Graphics_Cards)，是Manjaro官方wiki讲解有关于显卡配置问题的。