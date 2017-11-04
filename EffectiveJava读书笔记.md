---
title: EffectiveJava读书笔记
date: 2017-10-26 22:13:26
tags:
---

#EffectiveJava 读书笔记

本文于2017年10月26日再读一遍，对书中的内容进行整理提取，方便以后查阅

[TOC]

## 第一章：创建和销毁对象

### 创建对象的方法

  * 构造器方法new一个；
  * 公共静态方法返回实例；
  * Build工程模式。
  > 一般来说创建对象最简单的方法就是使用的时候new一个，如Person  person = new Person();既通过构造器创建对象。这种使用方法不适用于所有情况，例如一个单例工具类，不需要每次使用都new，而是通过一个公共静态方法返回类的实例。

#### 构造器方式
  *缺点*
  > 每一个构造器都与类名相同，无法知其寓意。

  构造器方法时最常用的方法；
#### 公共静态方法
  *优点1*
  > 每一个公共静态方法都有名字，顾名思义。

  *优点2*
  > 不必为每次调用都产生一个新的实例，可以重复利用。

  *优点3*
  > 可以返回原返回类型的任何子类型对象。

  *优点3*
  > 创建参数化类型实例，使代码变得更简洁。

  *缺点1*
  > 类如果不含有共有的或者受保护的构造器，就不能被子类化。

  *缺点2*
  > 与其他静态方法没有任何区别。

#### 使用情景1 通过方法名知其方法作用
  如BigInteger(int,int,Random)返回的BitInteger可能为素数，但如果用BigInteger.probablePrime静态方法来表示，更加清楚方法寓意。

  如果两个构造器方法中只是参数类型顺序上不同，很容易造成寓意混乱，需要进一步查询文档才能确定用哪一个，容错率很低。

#### 使用场景2 使用预先定义好的实例。
  公共静态方法返回的实例可以控制是否是单例,或者控制是否可以实例化。
  * getInstance和newInstance区别：
  > getInstance方法重点是返回已经创建好的实例。
    newInstance方法重点是返回一个新的实例。

#### 使用场景3 Collections的32个便利实现、通过适配器提供更丰富的服务接口
  分别提供了不可修改的集合、同步集合等。
  ```java
    // Service interface
    public interface Service{

    }

    //Service provider interface
    public interface Provider{
      Service newService();
    }

    public class Services{
      private Services(){};

      private static final Map<String,Provider> providers = new ConcurrentHashMap<String,Provider>();

      public static final String DEFAULT_PROVIDER_NAME = "<def>";

      public static void registerDefaultProvider(Provider p){
        registerProvider(DEFAULT_PROVIDER_NAME,p););
      }

      public static void registerProvider(String name,Provider p){
        providers.put(name,p);
      }

      public static Service newInstance(){
        return newInstance(DEFAULT_PROVIDER_NAME);
      }

      public static Service newInstance(String name){
        Provider p = provider.get(name);
        if(p == null){
          throw new IllegalArgumentException("No provider registered with name:"+ name);
        }
        return p.newService();
      }
    }
  ```

#### 使用场景4  代替繁琐的参数声明。

#### 使用场景6
  valueOf -- 返回的实例与参数具有相同的值，实际上是类型转换方法。
  of  -- valueOf的简洁替代。
  getInstance  -- 返回的实例是通过方法的参数来描述的，但是不能说与参数有相同的值。对于Singleton来说，该方法没有参数。
  newInstance  -- 跟getInstance一样，但是该方法确保返回的实例与其他实例不同。
  getType  -- 跟getInstance一样，但是在工厂方法中处于不同的类中的时候使用，Type表示工厂方法锁返回的对象类型。
  newType  -- 像newInstance一样，但是在工厂方法中处于不同的类中的时候使用。


### 构建器
  静态工厂和构造器都有个局限性，都不能很好的扩展到大量的可选参数。
  参数量大的情况下通常有两种方法：
  * 重叠构造器
    > 可行，但很难编写，难以阅读。

  * *工厂模式*
    > build构造参数很简单明了，链式调用很少出错。
      但是在构造Bean的时候，可能出现不一致的状态，类无法仅仅通过构造器参数的有效性来保证一致性。

### 用私有构造器或者枚举类型强化Singleton属性
  构造器私有，公开一个公共静态方法返回实例。
