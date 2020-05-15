---
title: 为老主板GA-H77-DS3H升级nvme支持
date: 2020-05-15 21:30:00
categories:
 - 笔记
tags:
---

为老主板GA-H77-DS3H加入NVME支持

<!--more-->

我的老电脑主板型号为GA-H77-DS3H，[GA-H77-DS3H 技嘉官网](https://www.gigabyte.com/tw/Motherboard/GA-H77-DS3H-rev-12/support#support-dl-bios)

首先到官网找到该主板的bios升级程序或固件，下载后，如果是exe，解包，提取出固件文件

选择了稳定版本的F10固件，解包后的到了`Efiflash.exe H77DS3H.F10  autoexec.bat`三个文件，我猜测`H77DS3H.F10`为固件

后将固件文件通过UBU(UEFI BIOS UPDATER)升级掉部分模块，以压缩BIOS固件体积，我选择的是升级SATA模块至最新，如视频中所说。然后有提到不推荐升级CPU Micro Code，以免出现旧处理器不兼容。（要真出这问题了，怕是还要要买颗亮机U降级bios

之后腾出空间后，使用**MMTool**将**NVME模块ffs文件**插入到**CSMCORE**中。这里导出的默认是**fd后缀**的文件。导出文件后，装载到**FAT32格式**的U盘中。

之后，插U盘，开机，DEL，进入BIOS升级程序，选择U盘，选择固件。耐心等待，重启。升级完毕。

硬盘转接卡还在路上，也不知道是否升级成功，中间重启了两次，不排除是bios升级失败，被自动还原。所以有待后续测试。

[GA-H77DS3H-F10 NVME模块插入后固件备份](https://release-1257799428.cos.ap-guangzhou.myqcloud.com/misc/GA-H77DS3H-F10-nvme.zip)

---
**参考资料：**

[【MOD BIOS】老主板上NVMe固态 - by 北极洲de企鹅](https://www.bilibili.com/video/BV1iW411W7mB)
