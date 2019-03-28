---
title: 在Linux上配置Windows远程桌面
categories:
  - 笔记
  - Linux
tags:
  - RDP
  - 远程桌面
  - Linux
  - Windows
  - rdesktop
date: 2019-03-28 11:09:00
thumbnail:
---
虽然已经将主力系统转为Linux，但是，有很多时候还是得用到Win系统的服务器，这时候就得靠远程桌面了。

<!--more-->

首先使用包管理器下**rdesktop**，Manjaro为例`$ sudo pacman -S rdesktop`。
这个包非常小巧，只有仅仅几M。接下来就可以直接使用命令行启动远程桌面了。

以我的为例

```bash
rdesktop -g <分辨率> -u <用户名> -p <密码> <服务器地址>
```

如果出现

>rdesktop ERROR: CredSSP: Initialize failed, do you have correct kerberos tgt initialized ? Failed to connect, CredSSP required by server

## 参数参考
>-u <username>
>Username for authentication on the server.
-n <hostname>
Client hostname. Normally rdesktop automatically obtains the hostname of the client.

---
**参考资料**

[Man Page](https://linux.die.net/man/1/rdesktop)

[逸鹏说道](https://www.cnblogs.com/dotnetcrazy/p/9709067.html)