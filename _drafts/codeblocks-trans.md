---
title: CodeBlocks 安装简体中文包(Linux)
date: 2019-01-21 11:38:03
thumbnail:
categories:
 - 笔记
tags:
 - CodeBlocks
 - Linux
---

在Linux上安装完CodeBlocks后。进行简体中文包安装。
这是CodeBlocks各种语言的语言包链接：
https://translations.launchpad.net/codeblocks/trunk/+pots/codeblocks

Status是一条绿色的进度条，代表着该项目的翻译进度。点击一下Status，按照进度排序，即可看到有一个**Chinese (Simplified)**排名十分靠前，点击进入找到**Download translation**，然后会提示登录Ubuntu帐号,这一点很烦人，没有的话就注册一个吧。点击进去后，会有两种格式的下载**MO**和**PO**,MO是PO文件编译后生成的文件。

>PO 是 Portable Object (可移植对象)的缩写形式；MO 是 Machine Object (机器对象) 的缩写形式。
>MO 文件是面向计算机的、由 PO 文件通过 gettext 软件包编译而成的二进制文件。程序通过读取 MO 文件使自身的界面转换成用户使用的语言。

我们需要的是MO文件。选择了**MO format**后，点击下面**Request Download**，接下来就是坐等收邮件了。

下载链接将会通过邮件发送到你的注册邮箱。收到邮件点击下载就可以啦。经过我的测试该链接貌似并不需要登录Cookie，所以这里我把链接放出来。希望没有那么快失效
官方链接：
http://launchpadlibrarian.net/407132629/zh_CN_LC_MESSAGES_codeblocks.mo
丢一个别人发的百度云链接备用（我也不知道这是什么版本的，可能不如我发的那个新）：
https://pan.baidu.com/s/1bpGkl0j?errno=0&errmsg=Auth%20Login%20Sucess&&bduss=&ssnerror=0&traceid=

找到你安装CodeBlocks的目录，我的是`/usr/share/codeblocks`,接下来你需要在**该目录下**创建目录`/locale/zh_CN`。如果没有权限，就用`sudo`。创建完后目录应该是类似我这样的`/usr/share/codeblocks/locale/zh_CN`,接下来就可以将刚刚下载好的**CodeBlocks.mo**文件拷贝过来了。可以使用`cp`命令。
> ## 笔记
> `cd`到要复制到的目录，例如执行`cp /home/weiyuan/Download/CodeBlocks.mo .`
>格式是`cp <被复制文件路径> <目标目录>`
>不要忘记上面最后一个点，代表复制至当前目录下。
复制完后，如果执行后续的步骤没有效果，试着给个777权限。
例如执行`sudo chmod 777 CodeBlocks.mo`。

做完以上步骤，重启你的CodeBlocks,在Settings->Environment->View(左栏)，找到右栏Internationalization（国际化），勾选，找到里面的**Chinese (Simplified)**选项。保存，重启CodeBlocks后，即可看到中文化后的界面了。

---
如果你想了解更多有关于语言包，已经国际化有关的内容，可以查看下面的链接。
扩展阅读：
[国际化与本地化](https://zh.wikipedia.org/wiki/%E5%9B%BD%E9%99%85%E5%8C%96%E4%B8%8E%E6%9C%AC%E5%9C%B0%E5%8C%96)
[Gettext](https://zh.wikipedia.org/wiki/Gettext)
[多国语言解决方案gnu.gettext + poedit](https://www.cnblogs.com/hont/p/5129806.html)