---
title: 浅析JVM内存结构
categories:
- JAVA
date: 2018-08-11 
tags:
- JVM
---
JVM把内存划分为五个区域。方法区（Method Area），堆区（Heap Area），栈区（Stack Area），PC寄存器（PC Registers），本地方法堆栈（Native Method stacks）·   

![](https://github.com/echolixiaopeng/blog/raw/master/data/JVM%20Runtime%20Data%20Areas.png)

### 方法区（Method Area）

* 方法区，是`线程共享`的。
* 方法区存放了要加载的类的信息，类中的静态变量、final定义的常量、类中的field、方法信息。
* `常量池`是方法区的一部分。
* 方法区对应`持久代`（Permanent Generation），在JDK1.8中`元空间`取代了持久代。
* 在`Full FC`时会回收方法区的空间。

### 堆区（Heap Area）
![](https://github.com/echolixiaopeng/blog/raw/master/data/jvm-heap.jpg)

* 堆区，是`线程共享`的。
* 堆区用来存储对象实例及数组值。
* 采用了分代管理方式，`年轻代`（Young Generation）和`老年代`（Old Generation）。
* 年轻代回收时触发`Young GC`，老年代回收时触发`Full GC`。
* 对象首先会在年轻代中分配，大对象直接在年老代中分配。
* 年轻代分为S0（Eden Space）和S1（Survivor Space）两部分。
* 年老代中存放在年轻代中经多次垃圾回收仍然存活的对象，和直接在年老代中分配的对象。
* -Xms：JVM堆初始分配内存。-Xmx ：JVM的最大堆分配内存。防止堆大小动态扩容，一般两者配置为相同参数。

### 栈区（Stack Area）

* 栈区，是`线程私有`的。
* 一个线程的每个方法在执行的同时，都会创建一个`栈帧`（Statck Frame）。
* 当方法被调用时，栈帧在JVM栈中入栈，当方法执行完成时，栈帧出栈。
* 栈帧中存储的有局部变量表、操作站、动态链接、方法出口等。

### PC寄存器（PC Registers）
* 寄存器，是`线程私有`的。
* 用于存储当前线程所执行的字节码的位置。

### 本地方法栈（Native Method stacks）
* 本地方法栈用于支持native方法的执行，存储了每个native方法调用的状态。

