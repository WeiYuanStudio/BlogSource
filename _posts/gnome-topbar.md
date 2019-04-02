---
title: Gnome桌面恢复旧式托盘
categories:
  - 笔记
  - Linux
tags:
  - Gnome
  - Linux
date: 2019-03-28 12:44:00
thumbnail:
---
最近在装**electron-小飞机**的时候，遇到了装完后，任务栏无法显示的问题。查找了一下，发现是新的Gnome桌面已经抛弃了旧式托盘，如果还想要显示的话需要安装一个Gnome插件。

<!--more-->

我的Manjaro已经是自带**gnome-tweak-tool**了的，如果没有这个包，要先去包管理器里，把这个包打一下

然后去下载[TopIcons Plus](https://extensions.gnome.org/extension/1031/topicons/)插件

下载到本地后解压，在解压后的目录中运行命令`make install`

下面是log

```bash
[adam@mibookpro Gnome]$ make install
glib-compile-schemas schemas
rm -rf _build
mkdir _build
cp -r --preserve=timestamps locale schemas convenience.js extension.js metadata.json prefs.js README.md _build
echo Build was successful
Build was successful
rm -rf ~/.local/share/gnome-shell/extensions/TopIcons@phocean.net
mkdir -p ~/.local/share/gnome-shell/extensions/TopIcons@phocean.net
cp -r --preserve=timestamps _build/* ~/.local/share/gnome-shell/extensions/TopIcons@phocean.net
rm -rf _build
echo Installed in ~/.local/share/gnome-shell/extensions/TopIcons@phocean.net
Installed in /home/adam/.local/share/gnome-shell/extensions/TopIcons@phocean.net
```

完成后重启一下系统。启动小飞机后就可以在任务栏上看到小飞机了。

---
参考资料
[Ubuntu 18.04 顶栏不显示坚果云图标解决方法，无需Unity桌面 By 故国有明 @开源中国](https://my.oschina.net/luochuan01/blog/1941181)