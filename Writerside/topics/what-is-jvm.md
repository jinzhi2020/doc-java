# 什么是 JVM

JVM 是 Java Virtual Machine 的首字母缩写，也就是 Java 虚拟机。这篇文档描述了什么是 JVM 的一些概要。通过这篇文档，可以初步了解 JVM 。

## 简介 {id="desc"} 

JVM 就是 Java 的虚拟机，通过软件模拟的具有完整硬件系统功能的、运行在一个完全隔离环境中的计算机系统。它是一个计算机系统，可以运行程序，不过 JVM 只能是运行 Java 的程序。

JVM 是通过软件来模拟 Java 字节码的指令集，是 Java 程序的运行环境。接着我们通过一张图来了解 JVM 的构成以及和我们编写的程序之间的关系:

<img src="http://file-linker.oss-cn-hangzhou.aliyuncs.com/yRkHzdkWpLyFKGw1QtLz.jpeg" alt="relation"/>

## JVM  是 Java 与具体平台的中间层

正是因为 JVM 的存在，作为一个中间层和具体的操作系统隔离开了。所以，我们说 Java 是和具体的平台无关的。通过下面这张图我们可以理解这一点:

<img src="http://file-linker.oss-cn-hangzhou.aliyuncs.com/HbtBUOe1QZYOOhEZknp4.jpeg" alt="middle_layer" />

## JVM 的主要功能

JVM 主要提供了下面这些能力:

1. 通过 ClassLoader 寻找并装载 class 文件
2. 解释字节码成为指令并执行，提供 class 文件的运行环境
3. 进行运行期间的内存分配和垃圾回收
4. 提供与硬件交互的平台

## JVM 的规范作用以及核心 {id="core"}

俗话说无规矩不成方圆，虚拟机也需要有相应的规范。这篇文档主要是对 JVM 的规范的概述。

### 什么是 JVM 规范 {id="what"}

Java 虚拟机规范为不同的硬件平台提供了一种编译 Java 技术代码的规范。这句话怎么理解呢？JVM 并不认识我们编写的 Java 代码，甚至我们不按照 Java 的语法来编写都没有关系。JVM 只认经过编译器编译的字节码，也就是说不管我们使用 Java 编写代码或者是 Kotlin 来编写，只要之后生成字节码是 JVM 认识的即可。所以，规范的存在与具体的语言无关。

JVM 规范使 Java 软件独立于具体的平台，因为编译是针对作为虚拟机的，而不是特定平台的。这一点和其他的编译型语言不一样，比如说 C、C++、Go、Rust。

另外，因为有 JVM 规范的存在，所以也可以针对这份规范有不同的虚拟机实现。但是接下去我们讲的都是官方实现的虚拟机。

### JVM 规范的主要内容 {id="content"}

VM 规范的主要内容，摘要如下:

<img src="http://file-linker.oss-cn-hangzhou.aliyuncs.com/Y41RDrYI4SQ469VPd1Lk.png" alt="context"/>

更多的内容可以参考[官方虚拟机的PDF文档](https://docs.oracle.com/javase/specs/jvms/se13/jvms13.pdf), 如下图:

<img src="http://file-linker.oss-cn-hangzhou.aliyuncs.com/0T4aqffpTXvNXZrClugr.png" alt="PDF"/>

