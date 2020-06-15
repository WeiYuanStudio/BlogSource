---
title: 操作系统课程笔记
date: 2020-04-21 00:00:00
tags:
---

大二上 操作系统 课程 笔记 

<!--more-->

## 操作系统介绍

- 处理器
- 设备管理器：分配外设
- 储存管理：分配内存空间
- 文件管理：文件系统
- 对以上资源的调度

## 系统特征

- 批处理：解决输入输出问题
- 单道程序系统：每次只调一个用户作业运行
- 多道程序系统：每次可调度多个作业运行
- 分时系统：多个用户同时使用同一台计算机，分时共享资源

## 执行方式

- 顺序执行 按照程序结构执行，封闭性，可再现
- 并发执行 执行时间宏观上并行，微观下单CPU串行，例如若使用了公共变量，将不可再现

程序不加控制的随意并发执行将引发问题，为此操作系统引入进程的概念，通过同步互斥机制解决。

程序并发执行失去封闭性的原因是共享资源的影响。假设程序`P(i)`的读变量集与写变量集分别为`R(i)`和`W(i)`，任意两个程序`P(i)`和`P(j)`可并发的条件为

保证程序两次读之间的数据不变化
- `R(i) ∩ W(j) = ∅`
- `W(i) ∩ R(j) = ∅`

保证写的结果不丢失
- `W(i) ∩ W(j) = ∅`

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

### 调度算法

- 轮转法
- 多级反馈轮转法
- 优先级法
- 最高响应比优先法 (HRRN)

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

linux内核在2.6.32版本后，优先让子进程运行，查阅资料基本回答都是由于父进程优先运行一般会操作内存，进而导致COW，需要页拷贝，优先运行子进程若子进程进行`exec`，即可避免COW，优化性能。

该特性可以在`/proc/sys/kernel/sched_child_runs_first`中修改，使用`echo`将一个非0值写入该文件，即可修改此特性。之后运行例程，你将得到不同的输出顺序

![linux-fork-flow](https://blog-1257799428.cos.ap-guangzhou.myqcloud.com/operating-system-note/linux-fork-flow.png-compress)

## pipe 管道

使用C语言在UNIX中使用pipe(2)系统调用时，这个函数会让系统构建一个匿名管道，这样在进程中就打开了两个新的，打开的文件描述符：一个只读端和一个只写端。管道的两端是两个普通的，匿名的文件描述符，这就让其他进程无法连接该管道。为了避免死锁并利用进程的并行运行的好处，有一个或多个管道的UNIX进程通常会调用fork(2)产生新进程。并且每个子进程在开始读或写管道之前都会关掉不会用到的管道端。或者进程会产生一个子线程并使用管道来让线程进行数据交换。

匿名管道特点：

- 只能用于具有亲缘关系的进程之间的通信（也就是父子进程或者兄弟进程之间）。
- 一种半双工的通信模式，具有固定的读端和写端。
- LINUX把管道看作是一种文件，采用文件管理的方法对管道进行管理，对于它的读写也可以使用普通的read()和write()等函数。但是它不是普通的文件，并不属于其他任何文件系统，只存在于内核的内存空间中

管道的使用：

1. 创建管道：由父进程调用pipe()创建管道
2. 创建子进程：父进程通过调用fork()创建子进程
3. 子进程与父进程确认方向：子进程与父进程各自断开无关端，比如父进程断开写端，子进程断开读端
4. 通信：写进程调用write()，读进程调用read()函数
5. 关闭管道：各自调用close()关闭管道


---
参考资料

[王道考研操作系统知识点整理](https://legacy.gitbook.com/book/wizardforcel/wangdaokaoyan-os/details)

[三歪听说你们需要操作系统](https://mp.weixin.qq.com/s/BWQB_0UrusiEwkPcUMs0vw)

[Does the Linux scheduler prefer to run child process after fork()? - Stack Overflow](https://stackoverflow.com/questions/23695915/does-the-linux-scheduler-prefer-to-run-child-process-after-fork)

[fork之后，父子进程的先后执行顺序如何反映？ - 知乎](https://www.zhihu.com/question/59296096)

[Linux下的进程通信方式：管道通信详解_好儿郎~志在四方-CSDN博客](https://blog.csdn.net/rl529014/article/details/51464363)

[管道 (Unix) - 维基百科，自由的百科全书](https://zh.wikipedia.org/wiki/%E7%AE%A1%E9%81%93_(Unix))
