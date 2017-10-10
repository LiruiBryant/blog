---
title: 深入分析Synchronized
date: 2017-09-27 11:34:18
tags:
---



# Java同步关键字（synchronzied）
   > 所有同步在一个对象上的同步块在同时只能被一个线程进入并执行操作。所有其他等待进入该同步块的线程将被阻塞，直到执行该同步块中的线程退出。
## 使用方法
* 实例方法同步    
   > 作用在实例上
   ```java
     public synchronized void add(int value){
    this.count += value;
     }
    ```
* 静态方法同步
   > 作用在对象上
   ```java
       public static synchronized void add(int value){
      this.count += value;
       }
    ```
* 实例方法中同步块
    > 作用在this这个实例上
    ```java
    public void add(int value){
        synchronized(this){
           this.count += value;
        }
      }
    ```
* 静态方法中同步块
    > 作用在MyClass这个class上
    ```java
    public class MyClass {
        public static synchronized void log1(String msg1, String msg2){
           log.writeln(msg1);
           log.writeln(msg2);
        }
        public static void log2(String msg1, String msg2){
           synchronized(MyClass.class){
              log.writeln(msg1);
              log.writeln(msg2);
           }
        }
      }
    ```


[参考]{http://ifeve.com/synchronized-blocks/}

[参考2]{http://cmsblogs.com/?hmsr=toutiao.io&p=2071&utm_medium=toutiao.io&utm_source=toutiao.io}