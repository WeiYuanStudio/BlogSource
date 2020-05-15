---
title: 操作系统课程笔记
date: 2020-04-21 00:00:00
tags:
---

大二上 操作系统 课程 笔记 

<!--more-->

### 线程与进程的区别

线程是一个进程内的基本调度单位。线程既可以由操作系统内核调度，也可以由用户程序控制

一个进程中可以有多个线程，他们共享程序、数据、和文件。独立使用各自的寄存器与堆栈。

进程是程序资源分配的基本单位，线程是CPU执行的基本单位。

进程拥有独立的地址空间，线程共享同一地址空间。

进程调度开销比线程大。

### 进程调度的时机

- 正在进行的进程执行完毕
- 正在进行的进程自我阻塞进入睡眠等待
- 正在进行的进程调用了P原语句，资源不足阻塞，或是调用了V原语句，激活等待执行的进程队列
- 正在进行的进程请求的I/O操作被阻塞
- 正在进行的进程的时间片耗尽
- 执行完毕系统调用，转而返回用户进程时
- 就绪队列中的某个进程优先度高于当前执行的进程

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

例程:

```c
#include <sys/types.h>
#include <stdio.h>
main() {
	int parent_pid = getpid(); //启动时的pid

	printf("start up root progress -> pid %d\n", parent_pid); //显示启动时的pid

	int pid0 = fork(); //第一次fork
	if (pid0 == 0) {
		/* 子进程 */
		printf("Child progress -> pid: %d  B \n", getpid());
	} else {
		/* 父进程 */
		printf("Parent progress -> pid: %d  A \n", getpid());
		int pid1 = fork();
		if (pid1 == 0) {
			printf("Child progress -> pid: %d  C\n", getpid());
		} else {
			printf("Parent progress -> pid: %d  A\n", getpid());
		}
	}
}
```

输出：

```text
➜  作业文档 git:(master) ✗ ./a.out 
start up root progress -> pid 10315
Parent progress -> pid: 10315  A 
Parent progress -> pid: 10315  A
Child progress -> pid: 10316  B 
Child progress -> pid: 10317  C
```

![linux-fork-flow](https://blog-1257799428.cos.ap-guangzhou.myqcloud.com/operating-system-note/linux-fork-flow.png-compress)

---
参考资料

[王道考研操作系统知识点整理](https://legacy.gitbook.com/book/wizardforcel/wangdaokaoyan-os/details)
