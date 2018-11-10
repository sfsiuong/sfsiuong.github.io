---
layout: post
title: 操作系统-用户级线程
date: 2018-11-9
tags: 笔记 操作系统
---

# 用户级线程（User threads）

`多个进程是如何切换的`

## 线程

-  进程 = 资源+指令执行序列

  + 将资源和指令执行分开

  + 一个资源+多个指令执行序列

    仅仅切换pc寄存器而不切换映射表 

**线程**（thread）  ：保留了并发的优点，避免了进程切换的代价

（分治的思想）

举个例子

打开一个网页，浏览器先加载出文字文本，后加载出图片，logo。

因为网页浏览器 不同线程处理不同的事情

- 一个线程从服务器接受数据
- 一个线程用来显示文本
- 一个用来处理图片
- 一个用来显示图片  

这些线程要共享资源

- 接受数据放在缓冲区
- 所有的图片，文本都显示在同一个屏幕上

开始实现这个浏览器

```
void WebExploerer()
{
    char URL[] = "http....";
    char buffer[1000];
    pthread_create(...,GetData,URL,buffer);
    pthread_create(...,show,buffer);
}
void GetData(char *URL,char *p){...};
void Show(char *p){...};
```

启动多个线程，分别调用函数

## Yield

两个线程共用了一个栈会出错误 ret不能切来切去

解决方法 用两个栈分别用于两个进程

[![1541746833594.png](https://i.postimg.cc/J4dN3KWw/1541746833594.png)](https://postimg.cc/nCB9p4zT)

两个线程的样子：两个tcb、两个栈、切换的pc在栈内