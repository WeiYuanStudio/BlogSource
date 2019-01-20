---
title: WebHook让站点服务器自动拉取测试
date: 2019-01-20 00:20:51
thumbnail:
categories:
 - 踩坑
tags:
---
由于博客迁移到了Hexo，写文章都是需要本地编辑好后，自己手动部署到站点服务器，还好Hostinger支持Git，这样可以大大减少工作量，不然每次更新都要在本地把public目录给打包，然后通过面板上传解包。前几天在更新博客的时候发现Hostinger面板的Git居然还是支持WebHook的，于是打算试一下WebHook自动部署。

<!--more-->

这个站点我一直是放在腾讯云开发者平台（前身Coding）的，因为在我开始用Hexo的时候，Github还没开放免费私有仓库。而腾讯云开发者平台是支持免费的私有仓库的。后来可能是由于微软收购了Github的原因。现在逐渐放开了，免费用户也可以创建私有仓库了。太棒了！

当我使用WebHook的时候也遇到了问题，在腾讯云开发者平台内我确信我正确设置了WebHook，可是一直出现400 Bad Request错误。貌似一直是无法正确POST。

![](https://ws1.sinaimg.cn/large/007i8nDUgy1fzcd9espsfj31hc0u0dlc.jpg)

```bash
响应头部
Date: Sat, 19 Jan 2019 15:39:02 GMT

Content-Type: text/plain; charset=utf-8

Content-Length: 12

Connection: keep-alive

Set-Cookie: __cfduid=XXXXXXXXXXXXXXXXXXXXXX; expires=Sun, 19-Jan-20 15:39:02 GMT; path=/; domain=.hostinger.com; HttpOnly

X-Content-Type-Options: nosniff

Expect-CT: max-age=XXXXXX, report-uri="https://report-uri.cloudflare.com/cdn-cgi/beacon/expect-ct"

Server: cloudflare

CF-RAY: XXXXXXXXXXXXX-XXX
```
然后我考虑了三种情况
* 我自己设置问题
* Hostinger的问题，可能是当前套餐不支持Git WebHook，或者是服务器在维护什么的
* 腾讯云的问题  

折腾了很久都无果，遂联系Hostinger客服。客服回复说我的当前套餐是支持WebHook的，Hostinger那边也没有对我进行限制，建议我按照官方文档来操作。。。。（这么简单的东西我还会操作错吗。看样子很有可能是Tencent的锅了，于是决定将项目迁移到Github并重新配置WebHook尝试。这篇文章就是在还没测试第一次Github推送前写的，如果写完了此文章push到Github后，站点自动更新了，就说明WebHook生效了。

现在我已经能看到站点自动更新了。

可见，是腾讯的锅了。将仓库迁移到Github后，使用Github的WebHook收到返回200。不再像腾讯一直返回400（已经测试过两种WebHook格式了，腾讯发出的WebHook，被一律返回400，不知道是不是因为国内网络特色的问题）

记录一下，Hostinger的Git自动部署**仅仅支持application/json**格式，不支持application/x-www-form-urlencoded格式。在Github上使用两种格式推送WebHook都可以成功收到200状态码返回。只不过使用x-www-form-urlencoded格式，服务器不会成功pull你所设定的仓库。