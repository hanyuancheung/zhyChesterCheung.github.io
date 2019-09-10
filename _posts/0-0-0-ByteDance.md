---
layout: post
title:  "ByteDance Interview"
categories: thinking
tags: thinking
author: Chester Cheung
---

* content
{:toc}


今天面试的公司是国内如日中天的大厂ByteDance字节跳动，听说字跳的面试都有3轮，比起之前面试的网易，深信服的一轮技术面要实在多了。这次一面也让我从整体上了解到了AI大厂在面试实习生的时候会问到的问题以及一些日后重点学习的方向。

1. 请你做个自我介绍

循规蹈矩的介绍......，顺便吸取上次面试的教训，不管他有没有让我说项目都一鼓作气把我觉得做的最好的项目都说出来了，然后等待面试官的下一步指示。

2. 看到你有写过网络聊天室，请问你在这个项目中的网络IO用到了什么IO框架，比如Java NIO之类的吗？

老实回答没有，因为缺失只是用到了最基本的IO流，对于框架暂时还没有涉及。经过查询后发现Java对于IO流的处理有很多成熟的框架，比如Netty等，java NIO(Non-blocking I/O)就是一种同步非阻塞的I/O模型，也是I/O多路复用的基础，是解决高并发与大量连接、I/O处理问题的有效方式。

3. 请问有用过Spring等大型产品通用框架吗？

4. 问些Java的问题吧，请问Java的多线程Thread的切换有哪些状态？

一共有5种状态：

A. 新建new：新创建了一个线程对象 

B.可运行runnable：线程对象创建后，其他线程调用了该对象的start()方法，该状态的线程位于可运行的线程池中，等待被线程调度选中，获得cpu的使用权

C.运行running：可运行状态的线程获得了cpu的时间片，执行程序代码

D.阻塞block：线程因为某种原因放弃了cpu的使用权，也即让出了time slice，暂时停止运行，直到线程进入runnable的状态才有机会再次获得cpu的time slice转到运行状态，其中阻塞分为3种情况：

(1) 等待阻塞：运行的线程执行wait()方法，jvm会将该线程放入等待队列

(2)同步阻塞：运行的线程在获得对象的同步锁时，若该同步锁被别的线程占用，则jvm会把该线程放入线程池lock pool中

(3)其他阻塞：运行的线程执行Thread.sleep()或者join方法，或者发出I/O请求，jvm会把他设为阻塞状态

E. 死亡：线程run，main方法执行结束，或者因异常退出了run方法，则该线程结束生命周期，死亡的线程不可复生。

5. 比如说Thread.sleep()和线程在执行过程中获取并等待锁Synchronized在线程状态切换上面有什么区别？

sleep()使当前线程进入停滞状态（阻塞当前线程），让出CUP的使用、目的是不让当前线程独自霸占该进程所获的CPU资源，以留一定时间给其他线程执行的机会; sleep()是Thread类的Static(静态)的方法，因此不能改变对象的机锁，所以当在一个Synchronized块中调用Sleep()方法是，线程虽然休眠了，但是对象的机锁并没有被释放，其他线程无法访问这个对象（即使睡着也持有对象锁）。

wait()方法是Object类里的方法；当一个线程执行到wait()方法时，它就进入到一个和该对象相关的等待池中，同时失去（释放）了对象的机锁（暂时失去机锁，wait(long timeout)超时时间到后还需要返还对象锁）；其他线程可以访问；wait()使用notify或者notifyAlll或者指定睡眠时间来唤醒当前等待池中的线程。wiat()必须放在synchronized block中，否则会在program runtime时扔出”java.lang.IllegalMonitorStateException“异常。

**综上，sleep()和wait()方法的最大区别是：sleep()睡眠时，保持对象锁，仍然占有该锁；而wait()睡眠时，释放对象锁。但是wait()和sleep()都可以通过interrupt()方法打断线程的暂停状态，从而使线程立刻抛出InterruptedException**

6. 我们现在用线程一般不会直接new Thread然后runnable这种方法去写，而是用一些线程池来管理，好处是什么？线程池你熟吗？

1、使用new Thread()创建线程的弊端：

+ 每次通过new Thread()创建对象性能不佳。

+ 线程缺乏统一管理，无法掌控线程数量，可能无限制新建线程，相互之间竞争，及可能占用过多系统资源导致死机。

+ 缺乏更多功能，如定时执行、定期执行、线程中断。

2、使用Java线程池的好处：

+ 对存在的线程进行有效管理，减少空闲线程的回收，可以提升性能。

+ 可有效控制最大并发线程数，提高系统资源的使用率，同时避免过多资源竞争，避免堵塞。

+ 提供定时执行、定期执行、单线程、并发数控制等功能。

7. Hashmap这种数据结构的内部实现是什么样的？为什么Hashmap比较快，时间复杂度是O(1)呢？

8. 请你说一下Java的类加载机制

9. 请你用Java写一个单例模式(设计模式)

10. 再问一个mysql的问题吧，请你写几个sql查询语句