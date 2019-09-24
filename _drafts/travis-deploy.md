---
title: Travis部署笔记
date: 2019-09-24 16:00:00
thumbnail:
categories:
tags:
---

简单记录一下Travis的部署过程

部署自动发布release文件

安装Ruby环境，完成了Ruby环境的安装之后。

使用Ruby包管理器gem安装Travis CLI工具。

```bash
gem install travis -v 1.8.10
```

由于历史原因，开源项目建议使用以.com为后缀的Travis地址。当我在部署的时候就遇到了Travis CLI工具的API地址还是.org的问题。

使用`travis setup releases`，Travis将会试图读取该路径下的GitHub地址的Tag，比如我的LightMonitor作业仓库，读取为WeiYuanStudio/LightMonitor。读取完之后，提示无法在`https://travis-ci.org`找到我的仓库。这是当然的，因为我是用的是.com为后缀的的Travis仓库。

这时候可以使用命令切换API地址

```bash
travis endpoint --set-default -e https://api.travis-ci.com
```

我运行的效果大概是这样

```bash
C:\Users\Adam\Documents\LightMonitor (maven -> origin)
λ travis endpoint --set-default -e https://api.travis-ci.com
API endpoint: https://api.travis-ci.com/ (stored as default)
```

接着使用命令`travis setup releases`

输入Github账号，直接自动获取Github API key，然后输入要在Release里面发布的文件路径。最后一个Encrypt API Key一定要选上，因为是公开仓库，这个选项会将GitHub的API Key 使用Travis的公钥加密，这样只有Travis才能解出原始的API Key

```bash
C:\Users\Adam\Documents\LightMonitor (maven -> origin)
λ travis setup releases
Username: WeiYuanStudio
Password for WeiYuanStudio: ******
File to Upload: target\lightmonitor-1.0-SNAPSHOT.war
Deploy only from WeiYuanStudio/LightMonitor? |yes| yes
Deploy from maven branch? |yes| no
Encrypt API key? |yes| yes
```
