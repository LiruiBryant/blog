---
title: SpringCloud使用docker部署注册eureka找不到地址
date: 2017-07-31 10:59:47
tags:
---

跟随大神学习SpringCloud的时候，在使用docker部署时，遇到了提供者注册不到eureka上去，[教程地址在此](http://blog.csdn.net/forezp/article/details/70198649#reply)

之后我搜了下docker进程间通信找到了一个**解决办法**：

我弄了一整天也是一直注册不进去，后来又搜了搜docker进程间通信，发现一个方法，
eureka-server部署的时候给一个名字： docker run --name eureka-server -p 8761:8761
server-hi中部署使用link参数 docker run --link eureka-server（server部署时赋予的名字）:eureka-server(配置中写的地址) ......
注册不进去的可以试试。