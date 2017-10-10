---
title: Spring中的事务管理
date: 2017-09-17 17:40:06
tags:
---

## 基本特征
  * 原子性
  * 一致性
  * 隔离性
  * 持久性
## 事务的分类
  * 声明式事务
  * 编程式事务

## 数据错误类型
  * 脏读(dirty read)
    > 当前有两个事务A和B，当A事务对数据进行更改，但是还没提交（会有缓存），此时被事务B读取，然后事务A因其他原因导致失败数据回滚，此时的事务A的操作都是失败的，对数据的操作也是失败的，但是B事务已经读取了A没提交的数据，导致B读到了错误的数据。

  * 不可重复读(no-repeatable-read)
    > 当前有两个事务A和B，A事务将会对数据进行两次操作，当A事务操作一次成功后，B事务此时对数据操作更改了值并提交，随后A事务再进行读取数据，发现数据已经被更改，造成A事务的数据混乱。

  * 幻读(phantom read)
    > 与不可重复读同样都是多次读数据不一致的问题，但是no-repeatable-read强调的是本身需要的数据集改变了， phantom read强调多次查询得出的条件数据集改变了。

## 事务的隔离级别
  * ISOLATION_DEFAULT 默认级别
  * ISOLATION_READ_UNCOMMIT   事务最低级别，允许其他事务看到此事务中未提交的数据。   **这种级别会导致脏读、幻读、不可重复读**
  * ISOLATION_READ_COMMIT   保证数据提交后才能被另外一个事务看到。  **这种级别可以防止脏读，但是不能防止不可重复读和幻读**
  * ISOLATION_REPEATABLE_READ       **这种级别可以防止脏读、不可重复读，但是不能防止幻读**
  * ISOLATION_SERALIZABLE     **最强级别，可以防止脏读、不可重复读、幻读**



## 事务的传播行为
  * PROPAGATION_REQUIRED 如果存在一个事务，就是用这个事务，如果没有事务，就开启一个新的事务
    ```java
      //事务属性 PROPAGATION_REQUIRED
      void methodA(){
          ...
          methodB();
          ...
      }
      //事务属性 PROPAGATION_REQUIRED
      void methodB(){
        ...
      }
    ```
  * PROPAGATION_SUPPORTS 如果存在一个事务，就用这个事务，如果没有事务，就非事务执行，但是对于事务同步管理器，PROPAGATION_SUPPORTS和不使用事务有少许区别。
    ```java
    //事务属性 PROPAGATION_REQUIRED
    void methodA(){
        ...
        methodB();
        ...
    }
    //事务属性 PROPAGATION_SUPPORTS
    void methodB(){
      ...
    }
    ```
    当通过methodA调用methodB时，methodB就是用methodA的事务，如果单独调用methodB的时候，methodB就非事务执行。
  * PROPAGATION_MANDATORY 如果存在一个事务，就是用当前事务，如果不存在事务，就抛出一个异常。
  * PROPAGATION_REQUIRED_NEW 开启一个新的事务，
    ```java
        //事务属性 PROPAGATION_REQUIRED
        void methodA(){
          doSomeThingA();
          methodB();
          doSomeThingB();
        }
        //事务属性 PROPAGATION_REQUIRES_NEW
        void methodB(){
        ……
        }
    ```
    相当于：
    ```java
        TransactionManager tm = null;
          try{
          //获得一个JTA事务管理器
          tm = getTransactionManager();
          tm.begin();//开启一个新的事务
          Transaction ts1 = tm.getTransaction();
          doSomeThing();
          tm.suspend();//挂起当前事务
          try{
            tm.begin();//重新开启第二个事务
            Transaction ts2 = tm.getTransaction();
            methodB();
            ts2.commit();//提交第二个事务
          }
          Catch(RunTimeException ex){
            ts2.rollback();//回滚第二个事务
          }
          finally{
           //释放资源
          }
          //methodB执行完后，复恢第一个事务
          tm.resume(ts1);
          doSomeThingB();
          ts1.commit();//提交第一个事务
        }
        catch(RunTimeException ex){
          ts1.rollback();//回滚第一个事务
        }
        finally{
        //释放资源
        }
    ```
    > 在这里，我把ts1称为外层事务，ts2称为内层事务。从上面的代码可以看出，ts2与ts1是两个独立的事务，互不相干。Ts2是否成功并不依赖于ts1。如果methodA方法在调用methodB方法后的doSomeThingB方法失败了，而methodB方法所做的结果依然被提交。而除了methodB之外的其它代码导致的结果却被回滚了。使用PROPAGATION_REQUIRES_NEW,需要使用JtaTransactionManager作为事务管理器。

* PROPAGATION_NOT_SUPPORTED 总是非事务执行，并挂起任何事务，也需要JTATransctionManager作为事务管理器
* PROPAGATION_NEVER  总是非事务执行，如果存在一个事务，则抛出异常。
* PROPAGATION_NESTED 如果一个活动的事务存在，则运行在一个嵌套的事务中. 如果没有活动事务, 则按TransactionDefinition.PROPAGATION_REQUIRED 属性执行。这是一个嵌套事务,使用JDBC 3.0驱动时,仅仅支持DataSourceTransactionManager作为事务管理器。需要JDBC 驱动的java.sql.Savepoint类。有一些JTA的事务管理器实现可能也提供了同样的功能。使用PROPAGATION_NESTED，还需要把PlatformTransactionManager的nestedTransactionAllowed属性设为true;而nestedTransactionAllowed属性值默认为false;
  ```java
      //事务属性 PROPAGATION_REQUIRED
      methodA(){
      doSomeThingA();
      methodB();
      doSomeThingB();
      }
      //事务属性 PROPAGATION_NESTED
      methodB(){
      ……
      }
  ```
  > 如果单独调用methodB方法，则按REQUIRED属性执行。如果调用methodA方法，相当于下面的效果：
  ```java
        Connection con = null;
      Savepoint savepoint = null;
      try{
       con = getConnection();
       con.setAutoCommit(false);
       doSomeThingA();
       savepoint = con2.setSavepoint();
       try{
           methodB();
       }catch(RuntimeException ex){
          con.rollback(savepoint);
       }
       finally{
         //释放资源
      }
        doSomeThingB();
        con.commit();
      }
      catch(RuntimeException ex){
        con.rollback();
      }
      finally{
       //释放资源
      }
    ```
    > PROPAGATION_NESTED 与PROPAGATION_REQUIRES_NEW的区别:它们非常类似,都像一个嵌套事务，如果不存在一个活动的事务，都会开启一个新的事务。使用PROPAGATION_REQUIRES_NEW时，内层事务与外层事务就像两个独立的事务一样，一旦内层事务进行了提交后，外层事务不能对其进行回滚。两个事务互不影响。两个事务不是一个真正的嵌套事务。同时它需要JTA事务管理器的支持。
    使用PROPAGATION_NESTED时，外层事务的回滚可以引起内层事务的回滚。而内层事务的异常并不会导致外层事务的回滚，它是一个真正的嵌套事务。DataSourceTransactionManager使用savepoint支持PROPAGATION_NESTED时，需要JDBC 3.0以上驱动及1.4以上的JDK版本支持。其它的JTA TrasactionManager实现可能有不同的支持方式。
    PROPAGATION_REQUIRES_NEW 启动一个新的, 不依赖于环境的 “内部” 事务. 这个事务将被完全 commited 或 rolled back 而不依赖于外部事务, 它拥有自己的隔离范围, 自己的锁, 等等. 当内部事务开始执行时, 外部事务将被挂起, 内务事务结束时, 外部事务将继续执行。
    另一方面, PROPAGATION_NESTED 开始一个 “嵌套的” 事务,  它是已经存在事务的一个真正的子事务. 潜套事务开始执行时,  它将取得一个 savepoint. 如果这个嵌套事务失败, 我们将回滚到此 savepoint. 潜套事务是外部事务的一部分, 只有外部事务结束后它才会被提交。
    由此可见, PROPAGATION_REQUIRES_NEW 和 PROPAGATION_NESTED 的最大区别在于, PROPAGATION_REQUIRES_NEW 完全是一个新的事务, 而 PROPAGATION_NESTED 则是外部事务的子事务, 如果外部事务 commit, 潜套事务也会被 commit, 这个规则同样适用于 roll back.
    PROPAGATION_REQUIRED应该是我们首先的事务传播行为。它能够满足我们大多数的事务需求。
