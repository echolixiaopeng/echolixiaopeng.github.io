---
title: 浅析JVM结构
categories:
- JAVA
date: 2018-08-30 
tags:
- JVM
---
JRE只是JVM（Java Virtual Machine）标准的一个实现，JVM负责分析字节码，编译代码和执行代码。了解JVM有助于我们写出更高效的代码，下面的内容有助于你了解到JVM的架构和JVM的不同组建。
    
### 既然说结构，必须先上图
![JVM-Architecture](https://github.com/echolixiaopeng/blog/raw/master/data/JVM-Architecture.png)

### JVM的组成是什么？
如上图所示，JVM由三个子系统构成。
> 类加载系统 （Class Loader Subsystem）
> 执行时数据区域（Runtime Data Area）
> 执行引擎（Execution Engine）

#### 类加载系统 （Class Loader Subsystem）
类加载系统处理过程包括`加载`和`链接`，并在类文件运行时，首次引用类时`初始化`类文件。
##### 加载（loading）
通过一个类的完全限定查找此类字节码文件，并利用字节码文件创建一个Class对象。
> Bootstrap Class Loader 
> Extension Class Loader
> Application Class Loader

在Java的日常应用程序开发中，类的加载几乎是由上述3种类加载器相互配合执行的，在必要时，我们还可以自定义类加载器。

Java虚拟机对class文件采用的是`按需加载`的方式，也就是说当需要使用该类时才会将它的class文件加载到内存生成class对象。

虚拟机在加载时，采用的是`双亲委派模式`。其工作原理的是，如果一个类加载器收到了类加载请求，它并不会自己先去加载，而是把这个请求委托给父类的加载器去执行，如果父类加载器还存在其父类加载器，则进一步向上委托，依次递归，请求最终将到达顶层的启动类加载器，如果父类加载器可以完成类加载任务，就成功返回，倘若父类加载器无法完成此加载任务，子加载器才会尝试自己去加载。

##### 链接（linking）
> 验证（Verify）：字节码验证器将验证字节码是否正确。
> 准备（Prepare) ：为类变量以初始值分配内存。
> 解释（Resolve）：符号引用替换为直接引用。

##### 初始化（initialization）
静态变量会被赋予初值，静态方法区会被执行

#### 执行时数据区域（Runtime Data Area）
运行时数据区可以划分为五个区域

> 方法区（Method Area）：线程共享，存储类级别的数据。
> 堆区（Heap Area）：线程共享，存储对象，对象中的变量和数组。
> 栈区（Stack Area）：线程私有，每个方法在执行的同时都会创建一个栈帧。
> PC寄存器（PC Registers）：线程私有，存储当前执行指令地址。
> 本地方法堆栈（Native Method stacks）：线程私有，存储本地方法信息。

#### 执行引擎（Execution Engine）

>解释器：解释字节码，当一个方法被调用多次时，每次都需要一个新的解释。
>JIT编译器：发现重复的代码时，编译整个字节码并将其更改为本地代码。
>垃圾收集器：收集和删除未引用的对象。