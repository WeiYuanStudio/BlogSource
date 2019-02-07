---
title: Mac 制作启动盘 重装系统
categories:
  - 笔记
  - MacOS
tags:
  - EI Capitan
date: 2019-02-06 11:55:16
thumbnail:
---

捡垃圾捡到的Mac Mini A1283原装系统版本为10.10.5(Yosemite)。个人想升级到该机器支持的最新系统版本。中途尝试使用[DiskMaker X](https://diskmakerx.com/)制作系统镜像，但是总是报错，制作失败。于是便有了下面这篇文章。

<!--more-->

![](https://ws1.sinaimg.cn/large/007i8nDUgy1fzwmftl37fj30gc09wjsy.jpg)

首先下载并安装[OS X El Capitan系统升级包](https://itunes.apple.com/cn/app/os-x-el-capitan/id1147835434?ls=1&mt=12)，以防链接失效，你也可以在[El Capitan的官网](https://support.apple.com/zh-cn/HT206886)找到上述链接。

![](https://ws1.sinaimg.cn/large/007i8nDUgy1fzwnba05smj30oj0hr0ym.jpg)

打开磁盘工具。选择你用于制作启动盘的磁盘。选择抹掉，格式为**MAC OS 扩展式**，设定一个相对简单点的命名例如我设置的`ei`
打开终端，复制粘贴一下命令

```
$ sudo /Applications/Install\ OS\ X\ El\ Capitan.app/Contents/Resources/createinstallmedia --volume /Volumes/<改为你的卷名，例如我的设置"ei"> --applicationpath /Applications/Install\ OS\ X\ El\ Capitan.app --nointeraction
```

接下来会提示

```
WARNING: Improper use of the sudo command could lead to data loss
or the deletion of important system files. Please double-check your
typing when using sudo. Type "man sudo" for more information.

To proceed, enter your password, or type Ctrl-C to abort.

Password:
```

确认无误后输入你的系统密码
如果安装顺利会返回如下

```
启动时请按住option
Erasing Disk: 0%... 10%... 20%... 30%...100%...
Copying installer files to disk...
Copy complete.
Making disk bootable...
Copying boot files...
Copy complete.
Done.
```
![](https://ws1.sinaimg.cn/large/007i8nDUgy1fzwmqnikbcj30qf0fbjtm.jpg)

接下来，关机。
按住**OPTION**按键（Win标准键盘请按住**ALT**，我还是在使用Win键盘，换键盘预定）

选择完启动盘刚开始启动读条的时候，风扇和打了鸡血一样。出风也是热的。而且不像WinPE那样，Mac镜像盘读条超慢。

![](https://ws1.sinaimg.cn/large/007i8nDUgy1fzwmb98t7bj33402c0hdt.jpg)
接下来按照图形界面步骤安装即可。（在安装最后可能会卡在“剩余一秒钟”的地方，这时候不要关机，耐心等下去就是了。直到最后，安装完毕后还会自动重启进入安装完毕的系统。）