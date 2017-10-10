---
title: java线程详解
date: 2017-09-27 11:49:38
tags:
---

## 线程状态
   * 新建状态(New) : 
       > 线程对象被创建后，就进入了新建状态。例如，Thread thread = new Thread()。
   * 就绪状态(Runnable): 
       > 也被称为“可执行状态”。线程对象被创建后，其它线程调用了该对象的start()方法，从而来启动该线程。例如，thread.start()。处于就绪状态的线程，随时可能被CPU调度执行。
   * 运行状态(Running) : 
      > 线程获取CPU权限进行执行。需要注意的是，线程只能从就绪状态进入到运行状态。
   * 阻塞状态(Blocked) : 
       > 阻塞状态是线程因为某种原因放弃CPU使用权，暂时停止运行。直到线程进入就绪状态，才有机会转到运行状态。阻塞的情况分三种：
     * 等待阻塞 -- 通过调用线程的wait()方法，让线程等待某工作的完成。
     * 同步阻塞 -- 线程在获取synchronized同步锁失败(因为锁被其它线程所占用)，它会进入同步阻塞状态。
     * 其他阻塞 -- 通过调用线程的sleep()或join()或发出了I/O请求时，线程会进入到阻塞状态。当sleep()状态超时、join()等待线程终止或者超时、或者I/O处理完毕时，线程重新转入就绪状态。
     * 死亡状态(Dead) : 线程执行完了或者因异常退出了run()方法，该线程结束生命周期。
   > 这5种状态涉及到的内容包括Object类, Thread和synchronized关键字。
    
   * Object类，定义了wait(), notify(), notifyAll()等休眠/唤醒函数。
   * Thread类，定义了一些列的线程操作函数。例如，sleep()休眠函数, interrupt()中断函数, getName()获取线程名称等。
   * synchronized，是关键字；它区分为synchronized代码块和synchronized方法。synchronized的作用是让线程获取对象的同步锁。

## 实现多线程的两种方式：Thread和Runnable
   * Runnable 是一个接口，该接口中只包含了一个run()方法。它的定义如下：
    public interface Runnable {
      public abstract void run();
    }
    Runnable的作用，实现多线程。我们可以定义一个类A实现Runnable接口；然后，通过new Thread(new A())等方式新建线程。
    Thread 是一个类。Thread本身就实现了Runnable接口。它的声明如下：
    public class Thread implements Runnable {}
    Thread的作用，实现多线程。
    
   ### Thread和Runnable的异同点：
   * Thread 和 Runnable 的相同点：
        > 都是“多线程的实现方式”。
   * Thread 和 Runnable 的不同点：
        > Thread 是类，而Runnable是接口；Thread本身是实现了Runnable接口的类。我们知道“一个类只能有一个父类，但是却能实现多个接口”，因此Runnable具有更好的扩展性。
    此外，Runnable还可以用于“资源的共享”。即，多个线程都是基于某一个Runnable对象建立的，它们会共享Runnable对象上的资源。
    通常，建议通过“Runnable”实现多线程！