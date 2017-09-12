---
title: Mysql引擎myisam和innodb的异同
date: 2017-08-16 09:35:37
tags:
---
## myisam和innodb不同之处

* 1事务的支持不同：
  * innodb支持事务；
  * myisam不支持事务；
* 2锁粒度
  * indodb行锁应用
  * myisam表锁
* 3存储空间
  * innodb既缓存索引文件又缓存数据文件；
  * myisam只缓存索引文件。
* 4存储结构
  * myisam数据文件的扩展名为.myd myData，索引文件的扩展名是.myi myIndex
  * innodb所有的表都保存在同一个数据文件里面 即为 .ibd
* 统计记录行数
  * myisam保存表的总行数，select count(*) from table 会直接取出该值
  * innodb没有保存表的中行书，select count(*) from table 就会遍历整个表，消耗相当大。