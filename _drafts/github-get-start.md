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

这篇文章面向于还没使用过GitHub，或者对GitHub不了解的用户。是一篇随处可见的GitHub入门~~指北~~指南。

<!--more-->

# 什么是GitHub

> [GitHub](https://github.com)是一个面向开源及私有软件项目的托管平台，因为只支持git 作为唯一的版本库格式进行托管，故名[GitHub](https://github.com)。
[GitHub](https://github.com)于2008年4月10日正式上线，除了git代码仓库托管及基本的 Web管理界面以外，还提供了订阅、讨论组、文本渲染、在线文件编辑器、协作图谱（报表）、代码片段分享（Gist）等功能。目前，其注册用户已经超过350万，托管版本数量也是非常之多，其中不乏知名开源项目 Ruby on Rails、jQuery、python 等。
> 2018年6月4日，微软宣布，通过75亿美元的股票交易收购代码托管平台[GitHub](https://github.com)。

> Git(读音为/gɪt/。)是一个开源的分布式版本控制系统，可以有效、高速地处理从很小到非常大的项目版本管理。Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。


以面向对象程序设计思想来打个不那么准确的比分，Git好比是一个类，而[GitHub](https://github.com)平台是Git的实现。当然，**只在本地使用Git控制版本也是非常强大的**。与远端同步只是其中一个功能。**Git的精髓是版本控制**类似[GitHub](https://github.com)的平台还有很多，比如说[GitLab](https://about.gitlab.com)，[Coding](https://coding.net)，[Bitbucket](https://bitbucket.org/)（这个平台的公司还做了十分友好的图形界面Git管理器 —— [Source Tree](https://www.sourcetreeapp.com)，刚刚入门Git可以试一试这个，很遗憾的是只有Win & Mac平台，木有Linux平台，Linux平台下，可以使用VS Code中的Git插件来图形化管理Git仓库）

当然GitHub也不仅仅是一个Git的远程仓库托管平台。在这里，诞生了许多深具影响力的开源软件，开源项目。



# GitHub入门使用

这里仅仅对如何查看GitHub仓库内的代码做介绍。下面若有一些功能点击后异常，或者遇到页面打不开，加载不全，请自行|禾斗|学|上、网。

强烈建议**使用电脑食用GitHub**,手机版用户请选择**使用桌面模式**打开（就是以电脑版网页显示）

以我自己的作业仓库为例，截两张图做示范。

[JavaHomework By WeiYuanStudio @ GitHub](https://github.com/WeiYuanStudio/JavaHomework) 仓库地址

![手机版查看桌面版](https://ws1.sinaimg.cn/large/007i8nDUly1g1ood66n74j30sg1kwtee.jpg)

![电脑版仓库全览](https://ws1.sinaimg.cn/large/007i8nDUly1g1oonglzw0j31y916410v.jpg)

## 如何在网页上直接查看源码

找你要的文件点开就OK了。在线预览，都有很易懂的代码高亮。

![](https://ws1.sinaimg.cn/large/007i8nDUly1g1onx3rco6j31y515x466.jpg)



**下面提到的RAW格式打开貌似需要|禾斗|学|上、网，写这篇文章的时候直连加载不出来，由于你懂的特色，不同时期的网络有不同的表现，不同运营商的网络又有不同的表现。学会XX上网，使用Google，弃用全是百家号垃圾广告的百度，是非常值得拥有的技能。** ~~百度：话说的这么绝，等着被我拒绝收录吧。~~

Q：不要代码高亮，我要像**文本文档一样查看代码**。

A：**点击页面上面的RAW**，即可显示**原始文本**。

Q：我想**下载这单页代码的源码**，不要整个仓库的压缩包

A：**点击页面上面的RAW**,像文本文档那样显示RAW文档后，快捷键**CTRL+S**(下载)，或者右键下载

## 如何打包整个仓库文件下载

当我们需要**下载他人仓库的整个仓库文件**的时候，可以**点击页面上绿色的Clone or download按钮**下载，如果电脑没有安装Git~~电脑已经装了Git的，可以关闭这篇文章了~~**直接选择HTTPS协议(Use HTTPS)**下载即可，这会下载这个仓库打包好的**zip压缩文件**，比如这个仓库可以下载我的实验报告和源码。

Q：为什么下载的README.md文档打开没有像网页那样的排版，还有一堆不知道是什么的符号在文字前面，包围着文字。

A：这是.md文件，即Markdown文件。详情可见下面关于[Markdown的说明](#Markdown)

## 历史查看

Git的精髓就是版本控制，可以清晰的了解自己之前的代码是怎么样的，随时随地可以回滚到历史版本，只要在想要“存档”的时候，提交一次commit，之后就可以大胆的修改自己的代码，而不用担心改崩代码，没法复原到上一个时间点。不仅如此，它还支持历史版本比对。

先说简单的历史版本查看  
回到仓库首页，就是这篇文章第二张图的页面，找到commits(图里标红了“历史提交”)

![](https://ws1.sinaimg.cn/large/007i8nDUly1g1op39p8gfj312a0j3tbn.jpg)

**点击右侧的< >图标**，即可查看仓库的历史版本。和使用仓库首页的最新版本仓库没有太大出入，不过多讲解。

接下来说说重要的亮点! 版本比对，点击页面上靠左的链接。即可查看历史改动对比

![](https://ws1.sinaimg.cn/large/007i8nDUly1g1oplykm0pj31qh0vh0xq.jpg)



....

## 使用Markdown编写文档

在GitHub中，Markdown被广泛适用与撰写代码文档，当你新建一个项目时，在仓库目录下放置**README.md**文件时，GitHub会自动将该文档解析并显示在页面下方。

![实验四实验报告截图](https://ws1.sinaimg.cn/large/007i8nDUly1g1omszuz2zj312z0n7tbo.jpg)

> Markdown是一种轻量级标记语言，创始人为约翰·格鲁伯（英语：John Gruber）。它允许人们“使用易读易写的纯文本格式编写文档，然后转换成有效的XHTML（或者HTML）文档”。这种语言吸收了很多在电子邮件中已有的纯文本标记的特性。  
> 由于Markdown的轻量化、易读易写特性，并且对于图片，图表、数学式都有支持，当前许多网站都广泛使用 Markdown 来撰写帮助文档或是用于论坛上发表消息。例如：GitHub、reddit、Diaspora、Stack Exchange、OpenStreetMap 、SourceForge等。甚至Markdown能被使用来撰写电子书。  
> 来自维基百科

![来自Wikipedia](https://ws1.sinaimg.cn/large/007i8nDUly1g1om0y0zcbj314n0xm453.jpg)

可以理解成Markdown这种标记语言是HTML的简写，而至于具体的显示，表现如何，取决于CSS。  
所以写代码文档的时候，可以离开鼠标不需要去使用图形界面操作字体大小的编辑，不再会出现一个文档，上文是宋体，写着写着下文旧变成微软雅黑的问题。不需要去记住正文标题，二级标题等等是几号字体。  
MD是一门标记语言，你在写文档的时候只需要标记这是几号标题，列表，引用，代码块即可。剩下的显示效果，全部由CSS决定。不论是简单的排版，还是通过Markdown语法绘制冗长的流程图，甚至是数学公式。都可以通过Markdown搞定。  
不论你是拥有强大的编辑器(例如VS Code，注意，不是VS)，还是只有区区一个文本文档，都可以随时随地的编写Markdown文档。如果你对Markdown语法不是那么熟悉，你甚至可以在浏览器里打开一个在线的Markdown编辑器[Editor.md](https://pandao.github.io/editor.md/)进行编写时的预览。  

![Editor.md截图](https://ws1.sinaimg.cn/large/007i8nDUly1g1omgdlkjpj31cd0wt131.jpg)

写代码文档，是时候尝试向臃肿的商业软件office & wps告别，转向免费开源的Markdown了。

# Fork

> 复刻（英语：fork，又译作派生、分支）是一个软件工程名词，发生在当开发人员从一个软件包拷贝了一份源代码然后在其上进行独立的开发，创建不同的软件。这个术语不只意味着版本控制上的分支，同时也意味着开发者社区的分割，是一种形式的分裂。
> 来自维基百科

在GitHub仓库主页面接近右上角的地方有一个Fork的按钮，这个按钮是用于将他人的GitHub公开仓库”复制”一份到自己的GitHub仓库，Fork了一份仓库之后，你就可以像操纵自己的仓库一样对这个仓库中的代码进行修改。  

对从他人仓库Fork来的仓库进行修改后，可以向源仓库**发起Pull Requests**（下面会介绍到）,也可以不向源仓库发起请求，另起门派，成为一个分支维护下去，或者是自己闭源开发。

当然进行以上操作的同时，请注意版权声明。比较常用的开源许可协议有GPL、BSD、MIT、Mozilla、Apache和LGPL。他们都有不同的要求。有些要求不可以闭源，有些要求不允许用于商业。

![开源许可协议对比图 By 阮一峰](https://ws1.sinaimg.cn/large/007i8nDUly1g1pby3ixquj30m80dw3zd.jpg)



## Pull Requests

Pull Requests,中文翻译过来就是拉取请求。简称PR，常常用帮忙维护他人的仓库。向他人的仓库提交代码。

通常的流程是，Fork一份他人的仓库到自己的GitHub仓库。然后通过Git Clone操作下载到本地，修改完毕后。Push到自己Fork来的仓库中，接着在GitHub中向原仓库所有者发起请求。

![一张蚂蚁金服项目的Pull Requests截图](https://ws1.sinaimg.cn/large/007i8nDUly1g1pcphdcgtj31570mnad7.jpg)

如果通过了源项目所有者的确认，一般来说，会将你的提交Merge（合并）到源仓库。如果所有者拒绝了请求，Pull Requests会进入Close状态。也就是这次请求失败了。所以建议在想为他人仓库提交更新前，建议先查看该仓库是否以前就有过类似的被Close掉的请求。所以为了不白忙活，最好是去issues中问一下。

## Issues

在Issues里面你可以向开发者提出用户反馈，报告Bug，讨论新功能的加入等等对项目有建设性的话题。

当然向项目所有者提出Issues前，请先看Wiki，README，以及历史中被Closed掉的Issues。避免提出重复的话题。

