---
title: 操作系统课程笔记
date: 2020-04-21 00:00:00
tags:
---

## linux fork操作

英文文档直接在linux中`man fork`即可显示

函数签名

```c
pid_t fork(void);
```

返回值：
 - 如果成功创建则返回子进程的PID（在父进程中的`fork()`返回）
 - 如果返回0，意味着当前进程是子进程（在子进程中`fork()`返回）
 - 如果返回-1，意味着`fork()`操作是发生了错误

---
参考资料

[王道考研操作系统知识点整理](https://legacy.gitbook.com/book/wizardforcel/wangdaokaoyan-os/details)
