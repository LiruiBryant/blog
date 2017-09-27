---
title: java8新增功能
date: 2017-09-18 21:34:40
tags:
---

[TOC]

# 接口中default关键字修饰方法可以增加默认实现。
  ```java
    interface Formula{
      default double sqrt(int a ){
        return Math.sqrt(a);
      }
    }
  ```

# Lambda表达式

```java
  Collections.sort(names,(a,b)->b.compareTo(a));
```

# 函数式接口
 ```java
    @FunctionInterface
    interface Coverter<F,T>{
      T convert(F from);
    }

    Convert<String,Integer> converter = (from) -> Integer.valueOf(from);
    Integer converted = converter.convert("123");

 ```
# 方法和构造函数引用

  * 静态方法引用
```java
    Convert<String,Integer> converter = Integer::valueOf;
    Integer converted = converter.convert("123");

```
  * 通过:: 关键字获取方法或者构造函数的引用

```java
    class Something{
      String startsWith(String s){
        return String.valueOf(s.charAt(0));
      }
    }
    Something something = new Something();
    Converter<String,String> converter = something::startsWith;
    String converted = converter.convert("java");
```
  * 引用构造函数
```java
    class Person{
      String firstName；
      String lastName；
      Person(){};
      Person(String firstName,String lastName){
        this.firstName=firstName;
        this.lastName=lastName;
      };
    }
    //新建工厂类
    interface PersonFactory<P extends Person>{
      P create(String firstName,String lastName);
    }
    //通过构造函数引用将所有东西拼到一起，通过手动实现。

    PersonFactory<Person> personFactory = Person::new;
    Person person = personFactory.create("earyant","Lee");
```

# 类库示例
## Predicates(断言)
  > 是一个布尔类型的函数，该函数只有一个输入函数，它实现了多种默认方法，用于处理复杂的逻辑动词。(and ,or,negate(否定))

```java
  Predicate<String> predicate = (s)->s.length()>0;
  predicate.test("earyant")  // true
  predicate.negate().test("earyant"); //false  

  Predicate<Boolean> nonNull = Objects::nonNull;
  Predicate<Boolean> isNull = Objects::isNull;

  Predicate<String> isEmpty = String::isEmpty;
  Predicate<String> isNotEmpty = isEmpty.negate();
 ```

## Functions
  > 接收一个参数，并返回单一的结果，默认方法可以将多个函数穿在一起(compose，andThen)：

```java
  Function<String,Integer> toInteger = Integer:valueOf;
  Function<String,String> backToString = toInteger.andThen(String::valueOf);
  backToString.apply("123");
```

## Suppliers
  > Supplier 接口产生一个给定类型的结果，与Function不同的是，Supplier没有输入参数。

```java
    Supplier<Person> personSupplier = Person:new;
    personSupplier.get();
```
