---
title: JVM中的逃逸分析
date: 2018-06-08 23:39:33
categories: JVM
tags: ["逃逸分析"]
photos:
 - https://sdw-1254060699.cos.ap-chengdu.myqcloud.com/bing_photos/20180608.jpg
---

<p class="description">A person who knows why to live can bear any how to live.－－Fredrick W. Nietzsche</p>
<!-- more -->

### 1.概念引入
逃逸分析(Escape Analysis)是目前Java虚拟机中比较前沿的优化技术。

逃逸分析的基本行为就是分析对象动态作用域：当一个对象在方法中被定义后，它可能被外部方法所引用，例如作为调用参数传递到其他地方中，称为方法逃逸。
Java 创建的对象都是被分配到堆内存上，但是事实并不是这么绝对，通过对Java对象分配的过程分析，可以知道有两个地方会导致Java中创建出来的对象并一定分别在所认为的堆上。这两个点分别是Java中的逃逸分析和TLAB（Thread Local Allocation Buffer）线程私有的缓存区。

### 2.基本概念介绍

逃逸分析，是一种可以有效减少Java程序中同步负载和内存堆分配压力的跨函数全局数据流分析算法。通过逃逸分析，Java Hotspot编译器能够分析出一个新的对象的引用的使用范围从而决定是否要将这个对象分配到堆上。

在计算机语言编译器优化原理中，逃逸分析是指分析指针动态范围的方法，它同编译器优化原理的指针分析和外形分析相关联。当变量（或者对象）在方法中分配后，其指针有可能被返回或者被全局引用，这样就会被其他过程或者线程所引用，这种现象称作指针（或者引用）的逃逸(Escape)。通俗点讲，如果一个对象的指针被多个方法或者线程引用时，那么我们就称这个对象的指针发生了逃逸。

Java在Java SE 6u23以及以后的版本中支持并默认开启了逃逸分析的选项。Java的 HotSpot JIT编译器，能够在方法重载或者动态加载代码的时候对代码进行逃逸分析
逃逸分析研究对于 java 编译器有什么好处呢？我们知道 java 对象总是在堆中被分配的，因此 java 对象的创建和回收对系统的开销是很大的。java 语言被批评的一个地方，也是认为 java 性能慢的一个原因就是 java不支持栈上分配对象。JDK6里的 Swing内存和性能消耗的瓶颈就是由于 GC 来遍历引用树并回收内存的，如果对象的数目比较多，将给 GC 带来较大的压力，也间接得影响了性能。减少临时对象在堆内分配的数量，无疑是最有效的优化方法。java 中应用里普遍存在一种场景，一般是在方法体内，声明了一个局部变量，并且该变量在方法执行生命周期内未发生逃逸，按照 JVM内存分配机制，首先会在堆内存上创建类的实例（对象），然后将此对象的引用压入调用栈，继续执行，这是 JVM优化前的方式。当然，我们可以采用逃逸分析对 JVM 进行优化。即针对栈的重新分配方式，首先我们需要分析并且找到未逃逸的变量，将该变量类的实例化内存直接在栈里分配，无需进入堆，分配完成之后，继续调用栈内执行，最后线程执行结束，栈空间被回收，局部变量对象也被回收，通过这种方式的优化，与优化前的方案主要区别在于对象的存储介质，优化前是在堆中，而优化后的是在栈中，从而减少了堆中临时对象的分配（较耗时），从而优化性能。

示例:性能分析
``` java
public class EscapeAnalysis {
    private static class Foo {
        private int x;
        private static int counter;
        public Foo() {
            x = (++counter);
        }
    }
    public static void main(String[] args) {
        long start = System.nanoTime();
        for (int i = 0; i < 1000 * 1000 * 10; ++i) {
            Foo foo = new Foo();
        }
        long end = System.nanoTime();
        System.out.println("Time cost is " + (end - start));
    }
}
```
未开启逃逸分析:
-server -XX:-DoEscapeAnalysis -XX:+PrintGC
>![image](https://sdw-1254060699.cos.ap-chengdu.myqcloud.com/2018060801.png)
结果：
>![image](https://sdw-1254060699.cos.ap-chengdu.myqcloud.com/2018060802.png)

开启逃逸分析:
-server -XX:+DoEscapeAnalysis -XX:+PrintGC
>![image](https://sdw-1254060699.cos.ap-chengdu.myqcloud.com/2018060803.png)
结果：
>![image](https://sdw-1254060699.cos.ap-chengdu.myqcloud.com/2018060804.png)

分析结果，性能提升7.87倍

原文链接：[Java 逃逸分析](https://mp.weixin.qq.com/s/M33nObwet5x0SqFN6w8tZw)。
