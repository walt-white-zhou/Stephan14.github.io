---
layout:     post
title:      "寻址方式"
subtitle:   "基础介绍"
date:       2018-04-30 12:00:00
author:     "邹盛富"
header-img: "img/addresss.jpeg"
tags:
    - 操作系统
---
### 立即寻址
操作数 = A  

#### 优点
节省时间
#### 缺点
数的大小受到地址字段的限制


### 直接寻址
EA = A  即指令里保存的是操作数的地址


### 间接寻址
EA = （A）即指令里包含了了存储器的一个地址，该地址所指向的空间里保存了操作数的地址
#### 缺点
需要两次访问存储器：第一次获取操作数的地址，第二次获取操作数的值


### 寄存器寻址
EA = R 类似于直接寻址，但是不是在内存中寻址，而是在寄存器中寻址
#### 优点
寄存器寻址的速度比直接寻址的速度快
#### 缺点
寻址范围比较小。

### 寄存器间接寻址
EA = （R）类似于间接寻址但是比间接寻址少访问一次存储器。

### 偏移寻址
EA = （R） + A 是类似于寄存器间接寻址 + 直接寻址。

- PC相对寻址： 使用的寄存器为PC程序计数器。一般这种寻址访问的都是相对靠近正在执行的指令，符合局部性概念。
- 基址寄存器寻址：就是说寄存器中保存了一个存储器的地址，然后地址字段中保存了相对于那个地址的偏移量。
- 变址：与基址寄存器寻址相反，就是说在地址字段里保存存储器的地址，然后再寄存器中保存相对于那个地址的偏移量。一般要求地址字段比较长，还有就是寄存器有专门的变址寄存器，会隐含的执行递增，递减操作。如果将通用寄存器当做变址寄存器来使用的话，需要指令中的某一位来通知自动变址。

    - 后变址：A中存的不是有效地址，需要对A进行间接寻址 EA = （R） +（A）
    - 前变址：R + （A）得到的不是有效地址，而是有效地址的地址。EA + （R + （A））


### 栈寻址
其实质是寄存器间接寻址，寄存器里存有栈顶指针。


### x86寻址方式
虚拟地址或有效地址又称为段位移，加上段的起始地址构成里一个线性地址。在虚拟存储器中，有效地址是虚拟地址或寄存器，把虚拟地址映射到物理地址是内存管理单元（MMU）的功能。如果采用了分页，该线性地址就要通过页转化机制来生成一个物理地址。段寄存器中保存了一个指向段描述符表（segment discription table）的索引，段描述符表存储于描述符寄存器。段描述符表存储了段的访问权限，段的大小和段的基地址（起始地址）。
