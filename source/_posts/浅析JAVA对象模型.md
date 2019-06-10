---
title: 浅析JAVA对象模型
categories:
- JAVA
date: 2018-07-15 
tags:
- JVM
---
Java对象在JVM中的存储也是有一定的结构的。而这个关于Java对象自身的存储模型称之为Java对象模型。

Java对象模型,在HotSpot JVM中即`OOP-Klass`模型。

`OOP`（Ordinary Object Pointer）指的是普通对象指针。使用new创建一个对象的时候，JVM会创建一个`instanceOopDesc`对象，保存在`堆`。

`Klass`用来描述对象实例的具体类型。每一个Java类，在被JVM加载的时候，JVM会给这个类创建一个`instanceKlass`，保存在`方法区`。


