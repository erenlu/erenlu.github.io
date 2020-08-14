---
title: 实时操作系统（RTOS）
index_img: https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200813011814.jpg
banner_img: https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200813011906.jpg
tags:
  - 学习笔记
  - 嵌入式
categories: 技术
abbrlink: 2cde
date: 2020-07-07 13:46:15
excerpt: 有关RTOS的定义和特点介绍。
---

> 最近要学习 TI 家的 CC2640R2F 板子，许多嵌入式知识都要恶补一通了。在此记录，以作梳理也便日后查阅。
>

# RTOS 的定义

RTOS (Real Time Operating Sysytem)，又称实时操作系统。一般而言，操作系统的任务就是管理计算机的硬件资源和应用程序。**实时操作系统（RTOS）是为实时应用提供服务，为需要执行的各种任务分配处理时间并按计划执行。**即当外界事件或数据产生时，能够在预定时间之内接受并予以处理，是控制所有实时任务协调一致运行的操作系统。因而，提供**及时响应**和**高可靠性**是其主要特点。

RTOS 按照实时性分为：硬 RTOS（必须使所有任务在规定时间内完成）和软 RTOS（能让大多数任务在确定时间内完成）。硬 RTOS 主要应用在对实时要求极高的工业领域，当然软硬 RTOS 之分又是不完全确定的，主要要根据具体项目需求进行设计。

构成：RTOS 由**“ kernel 内核”+“扩展模块”** 构成。其中 kernel 内核应满足实时系统的要求，扩展模块包含文件系统，网络协议栈，应用，设备服务驱动等。

![Real-time-Operating-System](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200812002227.png)

# 实时操作系统（RTOS）vs 分时操作系统（TSOS）

与实时操作系统相对应的是分时操作系统（Time-sharing Operating System，TSOS）。分时操作系统是把 [CPU](https://wiki.mbalib.com/wiki/CPU) 的时间划分成长短基本相同的时间区间，即“时间片”，通过操作系统的管理，把这些时间片依次轮流地分配给各个用户使用。分时操作系统的特点是能有效增加资源的使用率，专注最短时间尽可能地多进行计算。我们常见的 PC 电脑中 windows，macOs，Linux 都属于分时操作系统。

| 实时操作系统（RTOS）              | 分时操作系统（TSOS）                 |
| --------------------------------- | :----------------------------------- |
| 专注于快速响应时间                | 专注于计算吞吐量                     |
| 只嵌入需要实时响应的设备中        | 应用广泛                             |
| 使用分时段或事件驱动的设计        | 使用分时段设计以允许多个任务同时运行 |
| 与 TSOS 相比，RTOS 的编码更加严格 |                                      |

# RTOS带来的优势

* 降低开发难度，直接使用系统API，即可完成系统资源的申请、多任务的配合（基于优先级的实时抢占调度，同优先级的时间片调度)，以及任务间的通信等（如锁、事件等机制）。
* 系统划分为功能明确的任务，不依赖其他任务，增加代码可读性，易于维护和管理。
* 提升可移植性，对接不同芯片的工作由操作系统完成，应用开发者只需要关注 OS 层接口。
* 密集集成的中间件：中间件组件以任务和驱动的方式增加。他们使用 RTOS 提供的资源与其他任务通信。基于事件被 RTOS 调度。（中间件是位于嵌入式系统软件与应用程序直接的软件）

# 总结

RTOS 是运行在 MUC（Mirco Controller Unit）/ MPU（Micro Processor Unit）之上的，RTOS 是为确保确定性行为和及时响应时间及中断提供调度保证的一种操作系统。

# 参考资料

- [什么是 RTOS ，为什么使用 RTOS]( https://liteos.github.io/quick-start/intro/why-use-the-rtos.html#rtos-%E6%A6%82%E5%BF%B5)
- [什么是实时操作系统 (RTOS)？](https://www.ni.com/zh-cn/innovations/white-papers/07/what-is-a-real-time-operating-system--rtos--.html)
- [嵌入式学RTOS到底有哪些作用？](https://strongerhuang.blog.csdn.net/article/details/104853088)
- [RTOS系统与Linux系统的区别.]()