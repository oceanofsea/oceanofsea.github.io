---
layout: post
title:  "Java Garbage Collection"
date:   2016-03-16
categories: Java
excerpt: 
tags: Java GC
---

* content
{:toc}

## Motivation

第一次接触到Java的GC是在写实现实验的时候，内存总是不够用，于是我在跑完一组实验就调用一次GC。然后我发现时间长的无法忍受。最近陆陆续续接到一些面试，而GC和并发是两个必问的问题。想想也是，毕竟当你做大型项目开发的时候，这两者是逃不开的。

为什么Java GC会慢呢？因为GC的实现往往会stop the world. 也就是说，除了GC相关的线程，其他的线程处于等待状态。而GC优化很多时候就是在减少stop the world发生的次数和时间。

## GC设计原理

1. Most objects soon become unreachable
2. There are only a small number of references from old objects to new objects

Based on the above two assumptions, we can divid the objects into two groups, "young generation"(disappear soon, minor gc) and "old generation"(less gc, major gc).

![gc generation](/images/generation.png) 

Figure from [^1]

The permanent generation from the chart above is also called the "method area" and it stores classes or interned character strings.

顾名思义，young generation存放new objects，幸存的年轻人进入 old generation.

下面这个图展示了GC前和GC后的状态：

![gc process](/images/before-and-after-java-gc.png)

### GC for young generation

对于新生代，它又被分成三个部分，一个Eden（这名字取得），两个survivor space。刚创建的对象一般放入Eden。当GC发生时，将Eden里的幸存者移到一个survivor space里，当一个survivor满了，就对这个survivor space进行清理，将幸存者移到另一个survivor space里。当survivor space里历经多次清理依旧完全的对象将被送到old generation。

### GC for old generation

* Serial GC
	* 基于经典的mark and sweep的方式
		* mark the surviving objects in the old generation.
		* sweep those un-marked and leaves only the surviving ones behind.
		* 将heap清扫之后不连续的部分连续存放，挨在一起
	* suitable for a small memory and a small number of CPU cores.

* Parallel GC
	* Serial GC的多线程版本
* Parallel Old GC (Parallel Compacting GC)
* Concurrent Mark & Sweep GC  (or "CMS")
* Garbage First (G1) GC

[^1]: http://www.cubrid.org/blog/dev-platform/understanding-java-garbage-collection/

## Reference

