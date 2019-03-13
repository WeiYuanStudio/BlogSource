---
title: Google Chromebook 2013 安装Manjaro-i3wm版本
categories:
  - 笔记
  - Linux
tags:
  - Manjaro
  - i3wm
  - Chromebook
date: 2019-03-13 09:00:00
thumbnail:
---
F2+mod  `~/.i3/config` Chrome
Auto StartUp fcitx `~/.i3`

最近购入了Google Chromebook Pixel 2013, 原因是想多一台电脑来使用i3wm。而且对于这么一台笔记本。使用i3桌面能极大的提升续航能力。3代处理器的功耗和发热量感人。

<!--more-->

首先是关于分辨率方面的，在xfce， kde, gnome 等现代化桌面，这些调节都是十分之方便的，然而到了i3上面就不是那么方便了。使用快捷键`mod + ctrl + b`打开控制面板，选择**2. Display** -> **2. Set resolution** -> 选择显示器(笔记本显示器是**eDP1**) -> 选择分辨率 -> **18. 1280x850** 退出了2k屏后，一切显示都更加舒服了。但是字略有点虚。

返回上级菜单有一个7. Save display settings，不过这个并不能解决开机自启自动配置分辨率的问题。所以需要**autorandr**。在命令行中使用autorandr会发现没有找到这条指令，Manjaro-i3wm版本并没有附带这个软件包，所以需要自行安装一下这个包 `sudo pacman -S autorandr`，安装完毕后调节到合适的分辨率后可以使用`autorandr --save <配置名>`保存。当需要使用该配置的时候可以直接使用`autorandr --load <配置名>`来快速切换到该显示配置。所以接下来是将该配置加入到自启动文件当中。

本来作为一块2k屏，现在竟沦落到如此地步，不过实在是没有合适的分辨率1920x1275需要去xranr里面设置成虚拟分辨率。十分麻烦，而且在不调节DPI的条件下，1280x850是当前最合适的分辨率。若后期找到了更好的调节方式，将会更新一下。