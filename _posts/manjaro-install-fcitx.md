---
title: Manjaro 安装 fcitx 输入法
categories:
  - 笔记
  - Linux
tags:
  - Linux
  - Manjaro
  - fcitx
date: 2019-03-25 14:48:03
thumbnail:
---

随处可见的fcitx配置教程

<!--more-->

## 首先安装需要的软件包。

### 安装fcitx需要的几个组件

使用命令`$ sudo pacman -S fcitx-im` 打包安装fcitx需要的几个组件。

```
community/fcitx 4.2.9.6-1 (fcitx-im) [installed]
    Flexible Context-aware Input Tool with eXtension
community/fcitx-gtk2 4.2.9.6-1 (fcitx-im) [installed]
    GTK2 IM Module for fcitx
community/fcitx-gtk3 4.2.9.6-1 (fcitx-im) [installed]
    GTK3 IM Module for fcitx
community/fcitx-qt4 4.2.9.6-1 (fcitx-im) [installed]
    Qt4 IM Module for fcitx
community/fcitx-qt5 1.2.3-5 (fcitx-im) [installed]
```

## 安装图形界面的configure tool

一般来说，我们还需要图形界面的configure tool。所以这里我们再将configure tool也装上`$ sudo pacman -S fcitx-configtool`

## 安装输入法引擎
推荐使用谷歌拼音输入法引擎**fcitx-googlepinyin**和中州韵输入法**fcitx-rime**。
`$ sudo pacman -S <输入法引擎>`

谷歌拼音是从Android系统上面移植过来的一个引擎。很简单，啥都没有，安装体积小大概是他的优势吧。

中州韵输入法的话，词库稍稍比谷歌拼音引擎多一些。但是安装体积也大了很多，安装的时候看见完全安装体积80M。心疼了疼。如果是简体用户可以自行切换一下简体，快捷键Ctrl+grave(Tab上面那个键)。会弹出方案选单，找到适合自己的就OK了。我选择的是明月拼音-简化字。不知道这个是不是直接将繁体的词库直接转简体然后套来用的。至于会不会敲出港式，湾式拼音就不知道了。

拼音水平不是非常标准的话，还是推荐用搜狗输入法。汉语输入法纠错能力最强的只有搜狗输入法了。至于我个人的话，已经放弃使用毒瘤多年了，因为实在是无法忍受广告和卡顿。Win平台用微软官方输入法，Android用GBoard。虽然说输入速度个人觉得起码降了20%，但是还是觉得换来一个干净的系统环境，是值得的。

如果是中国镜像源的话，貌似还可以找到搜狗输入法引擎，不过不是非常推荐使用，毕竟Linux版也开始臃肿了。


## 配置启动文件

我们使用VIM编辑器来编辑路径于`~/.xprofile`文件。在里面写入下面这几行。

```
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
```

如果配置成功后，登出图形界面重新登录，或者重启后，将可以在输入区域正常切换输入法，并且可以看到输入框出现。（未成功配置好配置文件一般无法正常显示浮动输入框）

---
**参考资料**

[Fcitx@ArchWiki](https://wiki.archlinux.org/index.php/Fcitx_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))