---
title: 在Linux上安装Pycharm 解决没有图标的问题
tags:
  - 笔记
  - Linux
date: 2019-03-09 08:21:58
thumbnail:
categories:
---


最近突然对Python有点感兴趣。于是打算在自己的笔记本上也安装一个用于Python的IDE。IDE的话当然是坚持Jet Brians家的全家桶。

但是这次安装Pycharm不像之前那样，运行了`./pycharm.sh`发现并没有成功安装图标到启动器中。不可能说每次启动都去找那个启动.sh文件吧。而且终端一关Pycharm就GG了。

<!--more-->

仔细找了下资料发现并不难找到相关信息。
在我的Manjaro系统环境下， **.desktop** 文件放置在`/usr/share/applications`目录下。使用VIM编辑器你可以查看一下已经存在的 **.desktop** 文件，甚至可以恶搞地修改一下内容。

我在该目录下新建了一个 **pycharm.desktop** 文件，使用VIM编辑器填写如下内容。
```
[Desktop Entry]
Type=Application
Name=Pycharm
GenericName=Pycharm3
Comment=Pycharm3:The Python IDE
Exec=<这里填写启动软件的路径>
Icon=<这里填写图标路径>
Terminal=pycharm
Categories=Pycharm
```

这里摘抄一段ArchWiki的一段信息
>应用程序配置项，即 **.desktop** 文件是元信息资源和应用程序快捷图标的集合。系统程序的配置项通常位于 `/usr/share/applications` 或 `/usr/local/share/applications`目录，单用户安装的程序位于 `~/.local/share/applications` 目录，桌面环境会优先使用用户的配置项。

示范文件

```
[Desktop Entry]

# The type as listed above 填写应用类型
Type=Application

# The version of the desktop entry specification to which this file complies 符合的桌面条目版本
Version=1.0

# The name of the application 应用程序的名字
Name=jMemorize

# A comment which can/will be used as a tooltip 对应用程序的描述
Comment=Flash card based learning tool

# The path to the folder in which the executable is run 运行可执行文件的路径
Path=/opt/jmemorise

# The executable of the application, possibly with arguments. 可带参数的可执行文件 例如上面Pycharm 是使用.sh 来启动的
Exec=jmemorize

# The name of the icon that will be used to display this entry 图标名
Icon=jmemorize

# Describes whether this application needs to be run in a terminal or not 是否需要在终端中运行
Terminal=false

# Describes the categories in which this entry should be shown 分类描述
Categories=Education;Languages;Java;
```

所有键值可以在 [freedesktop.org 规范](https://specifications.freedesktop.org/desktop-entry-spec/desktop-entry-spec-latest.html#recognized-keys) 找到

---
参考资料
[Linux下将pycharm图标添加至桌面](https://blog.csdn.net/qq_36472696/article/details/75637163)