---
title: 跳过麦课安全网课
thumbnail: https://ws1.sinaimg.cn/large/007i8nDUgy1fuxtjkyz1bj30xc0laqgc.jpg
date: 2018-08-15 23:11:00
tags: 教程
---
PixivID:63640113 By:高原さと

最近学校搞了个并没有什么卵用的网课。F12打开面板研究了下源码，很快就有办法了。下面是跳过网课的教程。

<!-- more -->

## 1.电脑登陆微伴
![1.png][1]

## 2.按F12打开调试界面，调试界面切到控制台
![2.png][2]

## 3.选一课，点进去，在控制台下输入finishWxCourse()，然后回车。网页提示完成课程
![3.PNG][3]
![4.PNG][4]

## 4.OK，这时候返回上一页选课，反复刷课
连代码注释都不删的前端XD，不是好前端。看一遍就知道怎么跳过网课了。Orz


  [1]: https://wx3.sinaimg.cn/large/007i8nDUgy1fxy27d7q6pj311y0lcgmd.jpg
  [2]: https://wx2.sinaimg.cn/large/007i8nDUgy1fxy27oe73yj311y0lct9z.jpg
  [3]: https://wx3.sinaimg.cn/large/007i8nDUgy1fxy27wjj3vj311y0lc76f.jpg
  [4]: https://wx3.sinaimg.cn/large/007i8nDUgy1fxy2877d12j311y0lcmz5.jpg
