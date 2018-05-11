---
title: Guava 1.1基本工具
date: 2018-04-19 17:13:44
tags: Guava
---

# 1.1-使用和避免null

map允许null作为键，但只能有一个。
concurrentHashMap不允许null作为键。

## Optional

```java
  Optional<Integer> possible = Optional.of(5);
  possible.isPresent(); // returns true
  possible.get(); // returns 5
```

### 静态创建方法

-   of
-   ofNullable
-   empty
    ### 实例方法
-   isPresent
-   get
-   or (jdk中是orElse)
-   orNull
-   asSet

> 意义：
>     使用Optional除了赋予null语义，增加了可读性，最大的优点在于它是一种傻瓜式的防护。Optional迫使你积极思考引用缺失的情况，因为你必须显式地从Optional获取引用。直接使用null很容易让人忘掉某些情形，尽管FindBugs可以帮助查找null相关的问题，但是我们还是认为它并不能准确地定位问题根源。
>     如同输入参数，方法的返回值也可能是null。和其他人一样，你绝对很可能会忘记别人写的方法method(a,b)会返回一个null，就好像当你实现method(a,b)时，也很可能忘记输入参数a可以为null。将方法的返回类型指定为Optional，也可以迫使调用者思考返回的引用缺失的情形。

### 使用方式：

-   错误的使用方式：
    ```java
       Optional<User> user = Optional.ofNullable(user);
       if (user.isPresent()) {
         int sex = user.getSex();
         // 链式调用，最容易出现空指针
         int age = user.getParent().getParent().getParent().getAge();
       }
    ```
-   正确的使用方式
    ```java
     Optional<User> user = Optional.ofNullable(user);
     int sex = user.map(User::getSex).orElse(0);
     int age = user.map(User::getParent).map(User::getParent).map(User::getParent).map(User::getAge).orElse(0);
    ```

## 其他处理null的便利方法

-   Objects.firstNonNull(T, T)
-   Objects还有其它一些方法专门处理null或空字符串：emptyToNull(String)，nullToEmpty(String)，isNullOrEmpty(String)。

# 1.2-前置条件 Preconditions

| --                                                 | --                                                | --                        |
| -------------------------------------------------- | ------------------------------------------------- | ------------------------- |
| 方法声明（不包括额外参数）                                      | 描述                                                | 检查失败时抛出的异常                |
| checkArgument(boolean)                             | 检查boolean是否为true，用来检查传递给方法的参数。                    | IllegalArgumentException  |
| checkNotNull(T)                                    | 检查value是否为null，该方法直接返回value，因此可以内嵌使用checkNotNull。 | NullPointerException      |
| checkState(boolean)                                | 用来检查对象的某些状态。                                      | IllegalStateException     |
| checkElementIndex(int index, int size)             | 检查index作为索引值对某个列表、字符串或数组是否有效                      | IndexOutOfBoundsException |
| checkPositionIndex(int index, int size)            | 检查index作为位置值对某个列表、字符串或数组是否有效。                     | IndexOutOfBoundsException |
| checkPositionIndexes(int start, int end, int size) | 检查[start, end]表示的位置范围对某个列表、字符串或数组是否有效\*           | IndexOutOfBoundsException |











[参考](http://ifeve.com/google-guava-using-and-avoiding-null/)
