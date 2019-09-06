---
title: 在Linux上安装8051单片机开发环境
categories:
  - 笔记
  - 嵌入式
tags:
  - 8051
  - 踩坑
  - 嵌入式
  - 单片机
  - 开发环境
date: 2019-03-25 15:28:45
thumbnail:
---
# 安装SDCC编译器

**SDCC** 即是 **Small Device C Compiler**开源编译器，作为我们嵌入式开发的编译器 [SDCC官网](http://sdcc.sourceforge.net/)

作为Manjaro系统用户，使用包管理器`$ sudo pacman -S sdcc`安装sdcc

编译时直接使用`sdcc source.c`即可编译。编译完成后，工作目录下将出现一堆文件，asm啊ihx啊lk啊，什么的都有。接着使用`packihx source.ihx > source.hex`转换成**hex文件**

# 下载烧写工具

烧写工具可以选择stc官方工具，也可以选择开源的一个python脚本[stcflash](https://github.com/laborer/stcflash)来进行烧写。我在Win10上还未成功对PL2302的芯片进行过写入，不知道是不是驱动问题。建议在选择开发板的时候注意一下，尽量购买**CH340串口芯片**，而不是**PL2302串口芯片**。好像是因为PL2302盗版居多，而且官方驱动维护也有问题，所以历史遗留问题，最好选择CH340，这样驱动方面不容易出问题


如果遇到以下问题，可能是因为缺少Python包的原因

```
python stcflash\stcflash.py led.hex
Traceback (most recent call last):
  File "stcflash\stcflash.py", line 22, in <module>
    from serial import Serial
ImportError: cannot import name Serial
```

请使用**pip**安装**serial**和**pyserial**

---
**参考资料**
[SDCC编译器英文文档](http://sdcc.sourceforge.net/doc/sdccman.pdf)