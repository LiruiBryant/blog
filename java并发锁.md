---
title: java并发锁
date: 2017-09-27 11:37:55
tags:
---

## 实现方式
   * 锁
        -[ ] 公平锁
        -[ ] 非公平锁
        * 隐式锁Synchronized
        * 显式锁Lock
            * ReentrantLock
            * ReadWriteLock
            * ReentrantReadWriteLock
            * StampedLock 
        * 
   * 无锁
        * atomic
        * concurrent
        * blocking
        * threadLocal
        * volatile
        * CAS
        

## 锁
   * Synchronized
   ```java
    public class Counter{
    	private int count = 0;
    	public int inc(){
    		synchronized(this){
    			return ++count;
    		}
    	}
    }
   ```
   * Lock
   ```java
    public class Counter{
    	private Lock lock = new Lock();
    	private int count = 0;
    	public int inc(){
    		lock.lock();
    		int newCount = ++count;
    		lock.unlock();
    		return newCount;
    	}
    }
   ```
   不可重入自旋锁的简单实现
   ```java
    public class Counter{
    public class Lock{
    	private boolean isLocked = false;
    	public synchronized void lock()
    		throws InterruptedException{
    		while(isLocked){
    			wait();
    		}
    		isLocked = true;
    	}
    	public synchronized void unlock(){
    		isLocked = false;
    		notify();
    	}
    }}
   ```
    
## threadLocal
   如果想知道为什么使用ThreadLocal，就得先了解局部变量和全局变量对线程安全的影响。    
   局部变量存储在线程自己的栈中。也就是说，局部变量永远也不会被多个线程共享。所以，基础类型的局部变量是线程安全的。
   当多个线程引用同一个对象时，因为对象引用是放在栈上的，但是对象实例存储在堆中，所以不是线程安全的。
   
   ThreadLocal就是拷贝一份到线程缓存中，Thread正是操作这个对象，就不会出现安全问题。
   * InheritableThreadLocal
   
   InheritableThreadLocal类是ThreadLocal的子类。为了解决ThreadLocal实例内部每个线程都只能看到自己的私有值，所以InheritableThreadLocal允许一个线程创建的所有子线程访问其父线程的值。
   