---
layout: post
title: 操作系统-信号量-读者写者问题
date: 2018-11-14
tags: 笔记 操作系统
---

# 读者-写者问题

如果两个读者同时访问共享数据，那么不会产生什么不利的结果。

但是如果一个写者和其他线程同时访问共享图像，很可能会混乱。

----

要求写者对共享数据库有排他的访问

对于这个问题，读者与写者共享以下数据结构

```
semaphore mutex,wrt;
int readcount;
```

信号量`mutex`和`wrt`初始化为1；

`wrt`用于保证读者在读的时候，写者不能写。

`readcount`初始化为0；

mutex用于确保在更新变量readcount时的互斥；

readcount用来跟踪有多少读者，他为第一个进入临界区和最后一个离开临界区的读者使用；

*写者进程结构如下*

```
do{
    wait(wrt);
    ...
    signal(wrt);
}while (TRUE);
```

*读者进程结构如下*

```
do{
    wait(mutex);
    readcount++;
    if(readcount == 1)
    	wait(wrt);
    signal(mutex);
    ...
    //reading is performed
    ...
    wait(mutex);
    readcount--;
    if(readcount == 0)
        signal(wrt);
    signal(nutex);
}while(TRUE);
```

该问题可以进行推广用于对某些系统提供读写锁。

在以下情况最为有用：

- 当可以区分哪些进程只需要读共享数据而哪些进程只需要写共享数据。
- 当读者进程数比写进程多时。