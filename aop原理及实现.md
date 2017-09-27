---
title: aop原理及实现
date: 2017-09-17 15:48:32
tags:
---

## 静态代理、动态代理
1、AspectJ 使用静态代理，生成class文件时，会侵入到代码中。
2、Spring AOP 使用动态代理，不会侵入代码，而是在内存中临时为代码生成一个AOP对象，这个AOP对象包含目标对象的全部方法，并且在特定切点进行增强处理，并且调原对象的方法。
  * Jvm动态代理： 通过反射接收代理的类，并要求被代理的类必须实现一个接口。JVM代理的核心是InvocationHandler接口和proxy类。
  * CGLib(Code Generation Library)： 如果目标没有实现接口，那么Spring会使用CGLib动态代理目标类。它是一个代码生成的类库，可以在运行时动态生成某个类的子类。
      ```
      注意： CGLib是通过继承方式实现的，如果某个类被标记为final，是不能通过CGLib实现动态代理的。
      ```
## 示例(使用了CGLib)：
  定义Person类，其中sayHello方法是切点，切点被Timer注解修饰。
    ```java
      class Person{
        @Timer
        public void sayHello(){
          System.out.println("sayHello");
        }
      }
    ```
  配置切点：
  ```java
      @Aspect
      @Component
      class AdviceTest{
        @Pointcut("@annotation(com.earyant.aop.Timer)")
        public void pointcut(){

        }
        @Before("pointcut()")
        public void before(){
          System.out.println("before");
        }
      }
  ```
  在Mail类中调用。
    ```java
    class Main{
      @Autowired
      private Person person;
      public void main(){
        person.sayHello();
        System.out.println(person.getClass().getName());
      }
    }
    ```
  输出结果：
  ```
  before
  sayHello
  com.earyant.aop.Person$$EnhancerBySpringCGLIB$$56b89168
  ```
## 示例(JVM代理)
  定义一个接口：
  ```java
    interface Chinese{
      sayHello();
    }
  ```
  令Person类继承自Chinese类。
  ```java
    class Person implements Chinese{
      @Override
      @Timer
      sayHello(){
        System.out.println("sayHello");
      }
    }
  ```
  此代码运行的结果是：
  ```
    before
    sayHello
    com.sun.proxy.$Proxy53
  ```
  证明使用了JVM代理。
