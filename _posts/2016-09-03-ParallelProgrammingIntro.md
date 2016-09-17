---
layout: post
title: "Parallel Programming Introduction"
date: 2016-09-03 19:55
tags: ParallelProgramming
categories: ParallelProgramming
description: Some basic info about parallel programming
---

## Concurrency Computing, Parallel Computing, Distributed Computing

本来之前看过这类的文章，记得自己是懂了，但是今天偷听到有同学说之前写过多线程编程就是并行编程，又不由得想起来这个问题，这个确实是很容易混淆的两个概念，既然这个课是`Parallel Programming`，就先记录一下**并发（Concurrency）**和**并行（Parallel）**的联系和区别吧。

并行：多个核心分别同时处理一个或者多个任务。

并发：不同的任务看起来“同时”进行，实际上并不一定是同时的，可能是并行的，也就是不同的核心同时来进行这些不同的任务，也可能不是，只是单纯地相互穿插着进行，让人看起来这些任务是“同时”的。所以，并发包括并行，但是并发并不一定并行。

个人观点，在多核多任务的状态下，如果单个时间点上面只有一个任务在进行，其他任务被挂起，则是并发不是并行；如果单个时间点上所有的任务都在进行，那么就是并行。在单核状态下，多任务是并发，而不存在并行。

所以，多线程也不一定是并行的。

还有一个分布式计算，这个是对于程序来说的，是这个程序需要和其他的程序一起来解决一个问题。

下面是老师的原话：

- Concurrent computing – a program is one in which multiple tasks can be in progress at any instant.

- Parallel computing – a program is one in which multiple tasks cooperate closely to solve a problem.

- Distributed computing – a program may need to cooperate with other programs to solve a problem.


## Type of parallel systems

1. Shared-memory

![image]({{ site.baseurl | prepend:site.url}}/images/shared-memory.png){: .center-image }

2. Distributed-memory

![image]({{ site.baseurl | prepend:site.url}}/images/distributed-memory.png){: .center-image }

绝大部分的架构都是第一种（Shared-memory）


## The Von Neumann Architecture

![image]({{ site.baseurl | prepend:site.url}}/images/Von-Neumann-Architecture.png){: .center-image }

## A process and two threads

![image]({{ site.baseurl | prepend:site.url}}/images/1process-2threads.png){: .center-image }

## CPU Cache

当CPU将数据写进缓存中的时候，缓存中的数据可能和内存中的不同，有两种解决方法：

- Write-through Cache：将数据同时写进缓存和内存中，但是内存的速度远慢于缓存

- Write-back Caches：将数据立即写进缓存中，虽然速度快了，但是可能会在重写缓存区域时导致数据丢失

## Cache mappings（内存缓存映射）

- Full associative（全相连）：直接将内存中的数据放到缓存中，完全没有索引的作用，查找时从头查起，这种相联完全免去了索引的使用，而直接通过在整个缓存空间上匹配标签进行查找。 由于这样的查找造成的电路延迟最长，因此仅在特殊场合，如缓存极小时，才会使用。

- Direct mapped（直接映射）：所有的数据都有索引连接。

- n-way set associative（N路组相联）：上面两种是两种极端，这种方法将上面两种方法综合起来。使用组相联的缓存把存储空间组织成多个组，每个组有若干数据块。通过建立内存数据和组索引的对应关系，一个内存块可以被载入到对应组内的任一数据块上。
