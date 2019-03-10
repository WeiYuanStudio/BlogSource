---
title: 使用Shell脚本批量删除手机预装垃圾软件
date: 2019-03-10 09:03:58
thumbnail:
categories:
tags:
---


国产机买回来基本上全是垃圾预装。每次都要一个一个的点删除。当每次恢复出厂设置后，还要再删一遍。十分麻烦。对于我的主力机Mix2S，我选择了直接刷机。然而手头上有一台魅族手机。由于魅族的锁BL，魅族的机子几乎无法刷机。只能凑合着用。于是写了一个卸载脚本。方便每次重装完系统的清理。

<!--more-->

该脚本在Linux环境下编写运行。

首先，安装adb是必须的，具体的安装步骤，这里不再讲解。

拿到全新系统的手机，找到系统设置，将USB ADB调试给开了。这个选项在开发者选项里面。至于打开开发者选项，Flyme和MIUI都是在设置里面找到系统版本，连戳系统版本号打开的。

开了ADB调试后，打开电脑，运行一下 `ADB` 命令，测试一下环境是否配置成功。接着使用USB线连接上你的手机。如果没有问题的话，手机会弹出是否允许该计算机使用ADB之类的选项，确认后进行下一步。接下来使用 `adb devices` 后，应该可以看到设备列表里有你的手机出现。那串号码就是你手机的序列号。

```
$adb devices

List of devices attached
xxxxxxxxxxxx	device
```

能走到这一步，说明成功连接了手机，并开启了adb调试。

那么就可以使用下面的脚本来批量卸载了。

```shell
#!/bin/bash
#author: WeiYuan

echo "ADB批量卸载脚本"

for line in $(cat Data); do
    adb uninstall $line
done

```

复制以上内容，并保存为一个文件到干净的目录下。命名例如`Uninstall.sh`,然后在该空目录下运行命令`chmod +x ./Uninstall.sh`赋予脚本权限。脚本才能正常运行

接下来在这个目录里面新建一个文件名为`Data`的文件（不要后缀，和上面的脚本里面的文件名相同），这个文件里面写入要卸载的安装包。一行一个。如果懒人的话，可以选择使用`adb shell pm list packages` 显示所有的软件包，然后拷贝到Data文件里，使用文本编辑器把**package:**前缀给删除掉。只留包名，类似下面这样。

```
com.flyme.roamingpay
com.gd.mobicore.pa
com.android.providers.telephony
com.meizu.documentsui
com.android.providers.calendar
com.android.providers.media
com.meizu.flyme.hometools
com.mediatek.fwk.plugin
com.meizu.flyme.launcher
```

当然以上内容很多都是没有权限卸载的，真卸载了怕是还要开不了机。（所以建议在包名里找到想删的再编辑Data文件，不确定直接懒人法会不会砖，当然作为一台国产安卓机，砖了刷官方系统是非常简单的，官网上面都有放安装包，可以大胆一点尝试）

Data文件编辑完毕后，即可运行脚本`./Uninstall.sh`开始删除了。你可以看到垃圾预装的图标在一个一个减少。

![](https://ws1.sinaimg.cn/large/007i8nDUgy1g0xggbe7g1j30r80ix4a6.jpg)