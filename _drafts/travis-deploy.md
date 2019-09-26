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

输入Github账号，直接自动获取Github API key，（可以在personal access token中看到，权限只给了public_repo：Access public repositories）然后输入要在Release里面发布的文件路径。最后一个Encrypt API Key一定要选上，因为是公开仓库，这个选项会将GitHub的API Key 使用Travis的公钥加密，这样只有Travis才能解出原始的API Key。

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

完成后，项目目录下会生成一份`.travis.yml`文件，如果原来已经有了文件的话，会直接在后面添加

```yaml
language: java
jdk:
- openjdk11
deploy:
  provider: releases
  api_key:
    secure: MHzh0g0oI9AdZaKC+evibC+gfnDq8Dm0am+CzeoWEiFboXzxVRiJJWfdgZzbJiRcpb37jSYRWcnvAhAOWjzyT0aODQwgAWLbNlIe/tvHzO4Ly/MtMqBJUvQZaWeXe343+3gglZj5wCo8MmVbn6J43g1ljkyhjuF/r9JbDAuTGPfg6cADaPhcP+uCM03IKHF+YVSSElPVfLDLWMbHfFdlinSfMvcpmhT3wLMczY23bBxgxinEyTN1yp6F+HzF2mKma6FuW4eZtyu6gLAJqpSTwA3x4A7UMrtVVdw+7eJ+Yejrew7nPUMaJcwdAsHisF1YXSBiUlEbQsUESBIf/1vBmtwdX29LuoIVdGBXGQlrbx4pim9pM7Snv5qbRAMhFcyMATn6NScuf4gz+yOdeA4GiiWB0YaFXdR4OC7MLCVEKhN5KnyiIcteJdYWfdse9eXv2lbjpZxe5/Gx2Kc/gaJN9E6QCCgTFAJcp++x0b5yHnFe+PFmcpQPbI9x5uMIAdqFCdu+7/SkpoFEN/fbNGZEun0IPsn+p302b2qt6xGXIUA5PU8AzzNQOPmuqN2PDxvNuG4QzFPASMwFmHU3S2NjtIr1tkbpMa2bV5DblfS2RTPn9UR1u2knAgx9NZ88d0KVmqFWPzCPkAfwsefse1iOQTB/qWG8Hq8=
  file: target\lightmonitor-1.0-SNAPSHOT.war
  skip_cleanup: true
  on:
    tags: true
    repo: WeiYuanStudio/LightMonitor
```

很可惜，通过这种方式，我生成的密钥无法使用，Build完毕后最后的release阶段，总是提示401错误（因为key错误所导致）。

```bash
/home/travis/.rvm/gems/ruby-2.4.5/gems/octokit-4.6.2/lib/octokit/response/raise_error.rb:16:in `on_complete': GET https://api.github.com/user: 401 - Bad credentials // See: https://developer.github.com/v3 (Octokit::Unauthorized)
```

最后是去GitHub的API Key里面重置了这个由Travis CLI 工具生成的密钥。然后再使用Travis CLI工具将这个API Key加密导入

这里参考官方文档的操作[Encryption keys - Travis CI](https://docs.travis-ci.com/user/encryption-keys)

如果你使用的是 .com .org 的官方工具。首先你得登录Travis CLI工具，登录完毕后，使用`travis encrypt --pro SOMEVAR="secretvalue"`即可，我使用的时候没有参考官方文档。去Stack Overflow看了旧的帖子。他们的做法是直接`travis encrypt --org`（再加上参数`--add`会自动将密钥加入文件）（他们那时候还在用org后缀的站）然后等响应提示说输入GitHub API Key并按下Ctrl + D结束输入流。于是我照葫芦画瓢用了`travis encrypt --com`，然后到结束输入流的时候遇到问题了。。。万恶的Windows是以Ctrl + Z结尾的，这还不是最大的问题。用了Ctrl + Z还结束不了才是最骚的。遂去翻Travis在GitHub的Issues - [#7327](https://github.com/travis-ci/travis-ci/issues/7327)。不是我一个人遇到这种问题。PowerShell操作， cmd操作， 回车后Ctrl + D，回车后Ctrl + D 再回车，用`echo replacewithkeytoencrypt | travis encrypt`，场面之十分混乱。[晕]反正我是用了最后者解决的。可能就是因为这个Issue所以官方就改了，现在以参数形式输入密钥。

之后命令行会响应加密完成后的密钥，接着将这个密钥替换掉`.travis.yml`这个文件内的`deploy -> api-key -> secure`即可。虽然说GitHub会自动检查推送中内容是否无意泄露key，如果遇到泄露会自动屏蔽，并发邮件警告用户。但还是尽量注意一点，不要把未通过Travis密钥加密的Api-key给推上去了。

最后一步是修改Dockerfile内的配置，将下载war打包Release文件链接到Github Latest Release即可。参考我下面的链接，这大概算是一种GitHub的隐藏技能吧。我在Release页面根本找不到这个链接。

```Dockerfile
# Version 0.1

FROM tomcat:9.0.24-jdk11-openjdk
MAINTAINER WeiYuanStudio weiyuanstudio@gmail.com
RUN echo "Asia/shanghai" > /etc/timezone
RUN rm -rf /usr/local/tomcat/webapps/*
ADD https://github.com/WeiYuanStudio/LightMonitor/releases/latest/download/LightMonitor.war /usr/local/tomcat/webapps/ROOT.war
```

这样修改完后，每次推送Commit后，GitHub都会通过Webhook通知Travis CI和Docker Hub，仓库更新啦。然后Travis会拉取源码进行编译打包，完成后发布到仓库Release，接着Docker Hub的等待队列时间也差不多到了，拉取最新Release 中的war包，进行docker构建。

最后等待Docker镜像构建完成后，你就可以让自己的服务拉取最先的镜像进行部署了。赞！
