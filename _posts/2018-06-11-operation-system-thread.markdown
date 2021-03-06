---
layout:     post
title:      "线程"
subtitle:   "线程引入以及线程相关实现"
date:       2018-06-11 13:12:00
author:     "邹盛富"
header-img: "img/1525970676344.jpg"
tags:
    - 操作系统
---

### 线程的引入

#### 应用的需要
一个软件中同时工作多个任务，例如一个字处理软件需要同时需要编辑、排版、保存等线程同时工作

#### 开销的考虑
- 创建和撤销线程时间开销比较小
- 两个线程切换花费的时间比较少
- 线程之间的通信不需要内核介入

#### 性能考虑
多处理器可以提高性能

### 线程
线程是进程的一个运行实体，是CPU的调度单位，称为轻量级进程

#### 属性
- 有唯一标识符
- 有状态和状态转换
- 不运行时需要保存上下文（程序计数器等寄存器）
- 有自己的栈指针和栈
- 共享所在进程地址空间中和其他资源
- 创建或者撤销其他线程（进程刚创建的时候只有一个主线程，即单线程）

### 用户级线程
在用户空间建立线程库（例如：pthread库），提供一组管理线程的进程，线程的管理是通过运行时系统进行管理的，任何程序可以通过使用线程库被设计成多线程的程序。

#### 优点
- 线程切换比较快，不需要内核的支持，节省了两次状态转换（从用户态到内核态，再从内核态回到用户态）
- 调度算法是应用程序特定的，可以定制的，不扰乱底层的操作系统调度
- 用户线程可运行在任何操作系统上（只需要实现线程库）

#### 缺点
- 内核只将处理器分配给进程，同一个进程上的两个线程不能同时运行在两个处理器上，没有真正使用多处理技术
- 大多数系统调用是阻塞的，因此，由于内核阻塞进程，所以内核中的所有线程都会被阻塞

针对上述的问题可以使用jacketing技术来解决

### 内核级线程
内核对所有线程进行管理，并向程序提供API接口，内核要维护线程和进程的上下文信息，线程的切换需要内核的支持，同时调度也是以线程为单位进行的。

#### 优点
- 可以同时把一个进程内的多个线程调度到多个处理器中，如果其中的一个线程被阻塞，内核可以调度同一个进程内的另一个线程
- 内核例程自身可以使用多线程技术

#### 缺点
- 控制权从一个线程转换到另一个线程时需要通过内核转换

### 混合模型
- 线程的创建在用户空间完成
- 线程的调度在核心态完成
- 一个应用程序的多个用户级线程被映射到一些内核级线程上

Solaris就是采用这种模型，用户级线程和内核线程的比例为1：1
