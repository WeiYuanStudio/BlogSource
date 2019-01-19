---
title: 将旧域名重定向到新域名
thumbnail: https://ws3.sinaimg.cn/large/007i8nDUgy1fy08i7y2orj30xc0nl7wh.jpg
toc: 
date: 2018-12-9 8:21:00
categories:
 - 笔记
 - 网络
 - 建站
 - 踩坑
tags:
 - 网络
 - DNS
 - 建站
 - 重定向
 - CloudFlare
---
PS：
 - 今天是我的18岁生日，终于解锁了各种玩法，余额宝，蚂蚁花呗再也不会拒绝我这个小屁孩了。~~为什么关注点在这？~~（开通花呗看了看居然只有70额度，马云爸爸这是在施舍乞丐吗ヽ(#`Д´)ﾉ，不习惯玩文字游戏的我看了眼花呗的用户协议又把他关闭了
 - 珠海这边冷空气来了，好冷啊，qwq。头图就放一张[loudraw@Pixiv](https://www.pixiv.net/member.php?id=772547)大大的冷色调手绘吧
 - 这篇文章的主要目的是为了记录我踩坑的日常，以免日后又忘了怎么设置解析。(╥﹏╥)木有什么技术含量，大佬请直接右上角叉叉。（Mac用户请左上角叉叉，~~Smarti TNT用户请右下角叉叉，疯狂顽梗~~）
<!--more-->
#为什么要做重定向
由于迁移服务器前，站点一直使用的是`www.piclight.club`作为博客的地址，现在想要将`piclight.club`直接作为博客地址。（据说这样子做逼格高，逃～）其实是觉得自己的域名本来就很长了，再加上`www`就显得更加臃肿了，所以想去掉`www` 在我的CloudCone旧主机还没有过期的时候，`www.piclight.club`还是能正常访问的（旧站点），开通了在Hotinger的新主机后，我将`piclight.club`解析到了Hosting，也就是你现在看到的页面。在新站点完全搭建好后,决定做重定向。

![改动前的结构](https://wx2.sinaimg.cn/large/007i8nDUgy1fxzqdcqq9wj30qx0ijmxm.jpg)
![改动阶段](https://ws2.sinaimg.cn/large/007i8nDUgy1fxzqws5352j30r30ic3z8.jpg)
![最终效果](https://ws4.sinaimg.cn/large/007i8nDUgy1fxzqx5nppsj30qx0i7dgj.jpg)

原以为照着教程里面设置就可以成功的完成301重定向。谁知道，并没有成功。我试着打开`www.piclight.club`网页是无法访问状态。觉得应该是域名解析的缓存问题。想着这种事情，睡一觉起来就好了。于是去呼呼的睡午觉。醒来后发现依然没有成功。找了很久都没找到解决方案，网上基本上都是我所设定的设置那样。
又想起第一次搭建博客的那天晚上，折腾了半天，都没有将DNS解析设置正确。于是就睡了，认为是DNS解析缓存了，明天起来就能变好，谁知道一觉醒来，世界还是那个糟糕的样子。最终解决问题后，基本都是光速解析的(〒︿〒)  ~~菜的抠脚,倒是去怪DNS解析不够及时~~

![CloudFlare301](https://wx3.sinaimg.cn/large/007i8nDUgy1fxzr5crsa7j30xi0gd41p.jpg)

于是自己折腾了一下，发现尽管设定了301跳转，但是`www.piclight.club`在DNS中的解析依然不能删除。于是我重新将页面`www.piclight.club`的dns解析设置加了回去，只是这次没有使用A记录来将`www.piclight.club`指向旧的CloudCone站点。
![DNS](https://ws3.sinaimg.cn/large/007i8nDUgy1fxzrmkc5otj30xn0gaads.jpg)
而是使用了CNAME记录，将其地址指向了`piclight.club`（注意：这里使用CNAME记录的操作仅仅是相当于将`www.piclight.club`指向了新站点`piclight.club`的**IP地址**，并不会使浏览器跳转到新地址，真正起作用的还是靠在CloudFlare中**Page Rules**中的设置，将旧地址301（永久重定向）到新地址。图中打码部分因填入主机的IP地址）
至于为什么还要设置DNS记录，以我的环境是NAMECHEAP购买的域名DNS指向CloudFlare，由CloudFlare再指向我的主机。如果不设置旧地址的DNS的话。可能就会让旧地址置于根本没有解析去回应他的状态。设置之后，CloudFlare才会回应该地址的请求，并告诉访问者该地址已经迁移至新地址。
