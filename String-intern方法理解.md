---
title: String.intern方法理解
date: 2018-07-27 14:50:23
tags:
---
## intern作用
> 如果常量池中存在当前字符串, 就会直接返回当前字符串. 如果常量池中没有此字符串, 会将此字符串放入常量池中后, 再返回

先明白含义是什么，再看一道题，copy美团的文章，详见参考一
```Java
public static void main(String[] args) {
    String s = new String("1");
    s.intern();
    String s2 = "1";
    System.out.println(s == s2);

    String s3 = new String("1") + new String("1");
    s3.intern();
    String s4 = "11";
    System.out.println(s3 == s4);
}
```
> jdk7,8下false true
> jdk6以下false false

接下来把intern下移一行
```Java
public static void main(String[] args) {

    String s = new String("1");
    String s2 = "1";
    s.intern();
    System.out.println(s == s2);

    String s3 = new String("1") + new String("1");
    String s4 = "11";
    s3.intern();
    System.out.println(s3 == s4);
}

```
> jdk7,8下false false
> jdk6以下false false

看完答案后，我是很懵逼的，我有看了美团文章的解释，瞬间，更懵逼了。美团文章解释的其实挺好的，但是没有抓住重点。
详细论证两篇文章都有，基本都能看明白（看不明白可以找我交流），重点在两点：
* jdk7、8和6有什么不同： 7以上将常量池从perm区移到了heap中
* jdk7、8第一个程序中为什么一个是false，一个是true，**jdk7以后常量去不仅仅可以保存对象，也可以保存对象的引用**，所以s3的引用被保存到常量区中，s4直接在常量区找到了对象的引用，所以为true。


有兴趣研究的同学，可以联系我，微信 ryry89，邮箱earyantLee@gmail.com

[参考一](https://tech.meituan.com/in_depth_understanding_string_intern.html)
[参考二](https://www.cnblogs.com/Kidezyq/p/8040338.html)
