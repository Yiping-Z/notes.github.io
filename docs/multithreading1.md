---
layout: notes
title: 并发编程 1, 2 (lianglianglee)
description: 
image: 
---
创建线程方法
```java
public class RunnableThread implements Runnable {
public class ExtendsThread extends Thread {
```
Callable, 线程池

实现 Runnable 接口比继承 Thread 类实现线程要好
1. 实现了 Runnable 与 Thread 类的解耦，Runnable 里只有一个 run() 方法，它定义了需要执行的内容，Thread 类负责线程启动和属性设置等内容。
2. 使用继承 Thread 类方式，每次执行一次任务，都需要新建一个独立的线程，执行完任务后线程走到生命周期的尽头被销毁，如果还想执行这个任务，就必须再新建一个继承了 Thread 类的类。
使用实现 Runnable 接口的方式，就可以把任务直接传入线程池，使用一些固定的线程来完成任务，不需要每次新建销毁线程，大大降低了性能开销
3. Java 语言不支持双继承，如果我们的类一旦继承了 Thread 类，那么它后续就没有办法再继承其他的类，限制可拓展性

用 interrupt停止线程：请求中断，而不是强制停止，因为这样可以避免数据错乱，也可以让线程有时间结束收尾工作。
Thread.currentThread().isInterrupted()
如果 sleep、wait 等可以让线程进入阻塞的方法使线程休眠了，而处于休眠中的线程被中断，那么线程是可以感受到中断信号的，并且会抛出一个 InterruptedException 异常，同时清除中断信号，将中断标记位设置成 false。

已经被舍弃的 stop()、suspend() 和 resume()，它们由于有很大的安全风险比如死锁风险而被舍弃，而 volatile 这种方法在某些特殊的情况下，比如线程被长时间阻塞的情况，就无法及时感受中断，所以 volatile 是不够全面的停止线程的方法。
