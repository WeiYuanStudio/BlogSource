---
title: 一颗GitHub安利
categories:
  - 笔记
  - 环境
tags:
  - Git
  - GitHub
  - 代码管理
  - 版本管理
date: 2019-04-02 19:30:00
thumbnail:
toc: true
---

这篇文章面向于还没使用过GitHub，或者对GitHub不了解的用户。是一篇随处可见的GitHub入门~~指南~~指北。

<!--more-->

# 什么是GitHub

> [GitHub](https://github.com)是一个面向开源及私有软件项目的托管平台，因为只支持git 作为唯一的版本库格式进行托管，故名[GitHub](https://github.com)。
[GitHub](https://github.com)于2008年4月10日正式上线，除了git代码仓库托管及基本的 Web管理界面以外，还提供了订阅、讨论组、文本渲染、在线文件编辑器、协作图谱（报表）、代码片段分享（Gist）等功能。目前，其注册用户已经超过350万，托管版本数量也是非常之多，其中不乏知名开源项目 Ruby on Rails、jQuery、python 等。
> 2018年6月4日，微软宣布，通过75亿美元的股票交易收购代码托管平台[GitHub](https://github.com)。

> Git(读音为/gɪt/。)是一个开源的分布式版本控制系统，可以有效、高速地处理从很小到非常大的项目版本管理。 [1]  Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。


以面向对象程序设计思想来打个不那么准确的比分，Git好比是一个类，而[GitHub](https://github.com)平台是Git的实现。当然，**只在本地使用Git管理版本也是非常强大的**。与远端同步只是其中一个功能。**Git的精髓是版本管理**类似[GitHub](https://github.com)的平台还有很多，比如说[GitLab](https://about.gitlab.com)，[Coding](https://coding.net),[Bitbucket](https://bitbucket.org/)（这个平台的公司还做了十分友好的图形界面Git管理器 —— [Source Tree](https://www.sourcetreeapp.com)，刚刚入门Git可以试一试这个，很遗憾的是只有Win & Mac平台，木有Linux平台，Linux平台下，可以使用VS Code中的Git插件来图形化管理Git仓库）

使用GitHub的好处，不多说，看完介绍大概就懂了。

# GitHub入门使用

这里仅仅对如何查看GitHub仓库内的代码做介绍。下面若有一些功能点击后异常，或者遇到页面打不开，加载不全，请自行|禾斗|学|上、网。

强烈建议**使用电脑食用GitHub**,手机版用户请选择**使用桌面模式**打开（就是以电脑版网页显示）

## 如何在网页上直接查看源码

找你要的文件点开就OK了。在线预览，都有很易懂的代码高亮。

**下面提到的RAW格式打开貌似需要|禾斗|学|上、网，写这篇文章的时候直连加载不出来**

Q：不要代码高亮，我要像**文本文档一样查看代码**。

A：**点击页面上面的RAW**，即可显示**原始文本**。

Q：我想**下载这单页代码的源码**，不要整个仓库的压缩包

A：**点击页面上面的RAW**,像文本文档那样显示RAW文档后，快捷键**CTRL+S**(下载)，或者右键下载

## 如何打包整个仓库文件下载

当我们需要**下载他人仓库的整个仓库文件**的时候，可以**点击页面上绿色的Clone or download按钮**下载，如果电脑没有安装Git~~电脑已经装了Git的，可以关闭这篇文章了~~**直接选择HTTPS协议(Use HTTPS)**下载即可，这会下载这个仓库打包好的**zip压缩文件**，比如这个仓库可以下载我的实验报告和源码。

Q：为什么下载的README.md文档打开没有像网页那样的排版，还有一堆不知道是什么的符号在文字前面，包围着文字。

A：因为这是.md文件，即Markdown文件。详情可见下面关于[Markdown的说明](#Markdown)

## 历史查看

##　Markdown