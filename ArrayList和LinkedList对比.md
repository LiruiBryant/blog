---
title: ArrayList和LinkedList对比
date: 2017-09-10 17:12:21
tags:
---

## 接口
   * ArrayList ： 继承自List接口
   * LinkedList ： 继承自List和Deque接口
   
## 存储结构
   * ArrayList ：底层用数组进行存储
   * LinkedList ： 底层用双链表进行存储 
   
## 优点
   * ArrayList ： 访问速度快，插入和删除比较慢 ，需要移动元素
   * LinkedList ： 访问比较慢，插入和删除比较快 ， 需要链式查询