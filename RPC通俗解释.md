---
title: RPC通俗解释
date: 2017-07-18 09:55:06
tags:
---



# RPC通俗解释

## IPC和RPC


早些时间，一个电脑中多个线程互相独立，A线程和B线程都想用发送邮件功能，就需要开发两份代码，所以就有了一个协议是：IPC（Inter-process-communication）进行进程间通信，这样就可以在A中开发一个发送邮件的代码，B进程调用A进程代码就行了。
同比，现在的程序员们，为了方便调用其他电脑中的代码，研究出来RPC（Remote Procedure Call Protocol）；
