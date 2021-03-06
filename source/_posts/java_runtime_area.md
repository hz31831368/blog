---
title: Java虚拟机运行时数据区域
date: 2017/8/16
tags: Java虚拟机
---


Java虚拟机运行时数据区域主要分为方法区、Java堆、虚拟机栈、本地方法栈、程序计数器。其中方法区和Java堆一样，是各个线程共享的内存区域，而虚拟机栈、本地方法栈、程序计数器是线程私有的内存区。  

![image](http://on3qybwfn.bkt.clouddn.com/20170816001.png)

#### 程序计数器
程序计数器是一块较小的内存空间，它可以看作是当前线程所执行字节码的行号指示器。字节码解释器工作时就是通过改变这个计数器的值来选取下一条需要执行的字节码指令。

每条线程都需要一个独立的程序计数器来保证线程切换后能恢复到正确的位置，各条线程之间计数器互不影响，独立存储。     

如果线程正在执行的是一个Java方法，这个计数器记录的正式正在执行的虚拟机字节码指令的地址；如果正在执行的Native方法，这个计数器的值则为空（Undefined）。此内存区域是唯一一个在Java虚拟机规范中没有规定任何OurOfMemoryError情况的区域  

#### Java虚拟机栈
线程私有，生命周期与线程相同。虚拟机栈描述的是Java方法执行的内存模型：每个方法在执行的同时都会创建一个栈帧(Stack Frame) 用于存储局部变量表、操作数栈、动态链接、方法出口等信息，每一个方法从调用直至执行完成的过程，就对应这一个栈帧在虚拟机栈中入栈出栈的过程。

局部变量存放的数据类型有 
- 基本数据类型：boolean、byte、char、short、int、float、long、double。  
- 对象引用：reference类型，可能是一个执行对象的起始地址的引用指针，也可能是一个代表对象的句柄或其他与此对象相关的位置 。  
- returnAddress类型：指向了一条字节码指令的地址 。 

其中64位长度的long和double类型的数据会占用2个局部变量空间(Slot)，其余的数据类型只占用一个。局部变量所需的内存空间是完全确定的，在方法运行期间不会改变局部变量表的大小。  

在Java虚拟机规范中，对这个区域规定了两种异常状况：如果线程请求的栈的深度大于虚拟机所允许的深度，将抛出StackOverflowError异常；如果虚拟机栈可以动态扩展(当前大部分的Java虚拟机都可动态扩展)，如果扩展是无法申请到足够的内存，将会抛出OutOfMemoryError异常。  

<!-- more -->

#### 本地方法栈
本地方法栈与虚拟机栈所发挥的作用是非常相似的，他们之间的区别不过是虚拟机栈为虚拟机执行Java方法(也就是字节码)服务，而本地方法栈则为虚拟机使用到的Native方法服务。在虚拟机规范中对本地方法栈中方法使用的语言、使用方法与数据结构并没有强制规定，因此具体的虚拟机可以自由实现它。甚至有的虚拟机(HotSpot)直接就把本地方法栈和虚拟机栈合二为一。与虚拟机栈一样，本地方法栈区域也会抛出StackOverflowError和OutOfMemoryError异常。  

#### Java堆  
Java堆是Java虚拟机所管理的内存中最大的一块。它是被所有线程共享的一块内存区域，在虚拟机启动时创建。此内存区域的唯一目的就是存放对象实例，几乎所有的对象实例都在这里分配内存。  

Java堆是垃圾收集器管理的主要区域，从内存回收的角度来看，由于现在收集器基本都采用分代收集算法，所以Java堆中还可以细分为：新生代和老年代；再细致一点有Eden空间、From Survivor空间、To Survivor空间等。从内存分配的角度来看，线程共享的Java堆中可能划分出多个线程私有的分配缓冲区(Thread Local Allocation Buffer, TLAB)。不过无论如何划分，存储的仍然是对象实例，进一步划分的目的是为了更好地回收内存，或者更快地分配内存。  

Java堆可以处于物理上不连续的内存空间中，只要逻辑上是连续的即可。如果堆中分配内存失败会抛出OurOfMemoryError异常。  

#### 方法区
方法区是各个线程共享的内存区域，它用于存储已被虚拟机加载的类的信息、常量、静态变量、即时编译器编译后的代码等数据。不同于Java堆的是，Java虚拟机规范对方法区的限制非常宽松，可以选择不实现垃圾收集。但并非数据进入了方法区就“永久”存在了，这区域内存回收目标主要是针对常量池的回收和对类型的卸载。如果该区域内存不足也会抛出OutOfMemoryError异常。
- 常量池：这个名词可能大家也经常见，它是方法区的一部分。Class文件除了有类的版本、字段、方法、接口等描述信息外，还有一项信息就是常量池，用于存放编译期生成的各种字面量和符号引用。Java虚拟机运行期间，也可能将新的常量放入常量池（如String类的intern()方法）。
