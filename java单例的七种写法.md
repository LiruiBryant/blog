---
title: java单例的七种写法
date: 2017-09-27 14:06:24
tags:
---

# 懒汉
  ## 线程不安全
   ```java
  public class Singleton {  
      private static Singleton instance;  
      private Singleton (){}   
      public static Singleton getInstance() {  
        if (instance == null) {  
            instance = new Singleton();  
        }  
      return instance;  
      }  
 } 
```
## 线程安全
   ```java
    public class Singleton {  
      private static Singleton instance;  
      private Singleton (){}
      public static synchronized Singleton getInstance() {  
      if (instance == null) {  
          instance = new Singleton();  
      }  
      return instance;  
      }  
    }  
```

# 饿汉
   ```java
    public class Singleton {  
     private static Singleton instance = new Singleton();  
     private Singleton (){}
     public static Singleton getInstance() {  
     return instance;  
     }  
    }
```
## 饿汉变种
   ```java
    public class Singleton {  
      private static Singleton instance = null;  
      static {  
      instance = new Singleton();  
      }  
      private Singleton (){}
      public static Singleton getInstance() {  
      return this.instance;  
      }  
     }  
```

## 静态内部类
   ```java
    public class Singleton {  
          private static class SingletonHolder {  
          private static final Singleton INSTANCE = new Singleton();  
          }  
          private Singleton (){}
          public static final Singleton getInstance() {  
              return SingletonHolder.INSTANCE;  
          }  
      }  
```

## 枚举
   ```java
    public enum Singleton {  
         INSTANCE;  
         public void whateverMethod() {  
         }  
     } 
```

## 双重校验锁
   ```java
    public class Singleton {  
          private volatile static Singleton singleton;  
          private Singleton (){}   
          public static Singleton getSingleton() {  
          if (singleton == null) {  
              synchronized (Singleton.class) {  
              if (singleton == null) {  
                  singleton = new Singleton();  
              }  
             }  
         }  
         return singleton;  
         }  
     } 
```