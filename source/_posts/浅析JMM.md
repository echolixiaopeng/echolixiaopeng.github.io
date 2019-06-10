---
title: 浅析JMM
categories:
- JAVA
date: 2018-07-01 
tags:
- JVM
---
Java Memory Model（JMM），Java内存模型，是一个抽象的概念。JMM是围绕着多线程通信以及与其相关的一系列特性而建立的模型。JMM定义了一些语法集，这些语法集映射到Java语言中就是volatile、synchronized等关键字。

![](https://github.com/echolixiaopeng/blog/raw/master/data/jmm.jpeg)

JMM规定了所有的变量都存储在`主内存`（Main Memory）中。每个线程还有自己的`工作内存`（Working Memory）,线程的工作内存中保存了该线程使用到的变量的主内存的副本拷贝，线程对变量的所有操作（读取、赋值等）都必须在工作内存中进行，而不能直接读写主内存中的变量（volatile变量仍然有工作内存的拷贝，但是由于它特殊的操作顺序性规定，所以看起来如同直接在主内存中读写访问一般）。不同的线程之间也无法直接访问对方工作内存中的变量，线程之间值的传递都需要通过主内存来完成。

Java内存模型是围绕着并发编程中`原子性`、`可见性`、`有序性`这三个特征来建立。

#### 原子性
* 一个操作不能被打断，要么全部执行完毕，要么不执行。
* 基本类型数据的访问都是`原子操作`。
* 32位JVM中，long 和double类型的变量的读写操作是非原子操作。

#### 可见性
* 一个线程对共享变量做了修改之后，其他的线程立即能够看到（感知到）该变量这种修改（变化）。
* Java内存模型是通过将在工作内存中的变量修改后的值同步到主内存，在读取变量前从主内存刷新最新值到工作内存中，这种依赖主内存的方式来实现可见性的。
* volatile，synchronized，Lock，final关键字能实现可见性。
* volatile：每次使用volatile变量前立即从主内存中刷新，修改后的新值立刻同步到主内存。
* synchronized：同步块开始时，使用共享变量时会从主内存中刷新变量值到工作内存中；同步块结束时，将工作内存中的变量值同步到主内存中去。
* Lock接口：最常用的实现ReentrantLock(重入锁)来实现可见性。

#### 有序性
* 在本线程内观察，操作都是有序的；如果在一个线程中观察另外一个线程，所有的操作都是无序的。
* 无序指“`指令重排`”现象和“工作内存和主内存同步延迟”现象。
* volatile和synchronized来保证多线程之间操作的有序性。

#### happens-before原则
* 解释：A happens-before B，A操作执行的结果对B操作可见。
* `程序顺序规则`： 一个线程中的每个操作，happens-before于该线程中的任意后续操作。
*  `监视器锁规则`：对一个锁的解锁，happens-before 于随后对这个锁的加锁。
*  `volatile变量规则`：对一个 volatile域的写，happens-before于任意后续对这个volatile域的读。
*  `传递性`：如果 A happens-before B,且 B happens-before C,那么A happens-before C


