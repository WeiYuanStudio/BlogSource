---
title: KMS设置
categories:
  - 笔记
  - Windows
  - 系统
tags:
  - KMS
  - Windows
  - 系统
date: 2019-01-17 21:54:08
thumbnail:
---

关于KMS的解释，[维基百科](https://zh.wikipedia.org/wiki/%E5%A4%A7%E9%87%8F%E6%8E%88%E6%AC%8A%E9%87%91%E9%91%B0)就能看到了。我也就不再说明了。简单的说就是用于企业批量激活的。  
对于普通用户来说，可以利用KMS干净地，快捷地激活Windows。而不需要担心各种激活器带来的病毒风险。  
<!--more-->
下面开始操作，请使用**管理员权限**打开CMD或者PowerShell 

## 系统转VL版本

如果你的系统非VL版（批量激活版），请到[微软官网](https://docs.microsoft.com/en-us/windows-server/get-started/kmsclientkeys)找到你使用系统版本对应的VL密钥，将你的系统版本转换为VL版。转换VL密钥，你可以直接到系统设置里的激活设置里填入对应的VL密钥并确认，或者直接简单粗暴在命令行里运行

```bash
slmgr /ipk <此处为你需要转换的VL密钥>
```

## 开始激活

```
slmgr /skms <KMS服务器地址>
slmgr /ato
slmgr /xpr
```

关于KMS服务器地址，众所周知的原因，请自行搜索。最好是你能拥有一台支持开启KMS服务器的路由器。这样KMS服务器地址填入你的路由器网关即可，而且不用担心公网服务器崩掉，导致无法续期。（KMS激活需要每180天内续期一次。）
![](https://ws1.sinaimg.cn/large/007i8nDUgy1fz9ycnlhsdj31hc0u0adp.jpg)
我觉得，每一个爱折腾的人，都应该有一台能刷机的路由器。KMS激活服务可以说是绝大部分的自制路由器固件拥有的功能。想刷路由器的话，逛逛[恩山论坛](https://www.right.com.cn/forum/)，小学生都能学会。