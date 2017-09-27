---
title: java整理
date: 2017-09-09 21:36:59
tags:
---
## 算法和数据结构（性能，场景）：
### 数据结构：
  * 数组
  * 链表
  * 树
    * 红黑树
    * AVL树
    * Hash树
    * tire树
    * b-树
    * b+树
  * 队列
  * 栈
### 算法：
  * 查找：
    * 二分查找，以及变种二分查找。
  * 排序：
    * 7种排序
    * 时间、空间复杂度（理解并分析）
    * 动态规划、贪心算法
    * 图
### 计算机网络
  * OSI7层协议（TCP四层）
    * URL到页面的过程
  * http:
    *  http/https 1.0、1.1、2.0
    * get/post 以及幂等性
    * http相关头协议
  * 网络攻击（CSRF、XSS）
  * TCP/IP
    * 三次握手、四次握手
    * 拥塞控制（过程、阙值）
    * TCP与UDP比较
    * 子网掩码
    * DDos攻击
  * BIO、IO、NIO、AIO
    * 原理
    * Netty
    * linux内核的select poll epoll
### 数据库(mysql)
  * 索引
    * 分类
        * 全文索引
        * HASH索引
        * BTree索引
        * RTree索引
        * 普通索引
        * 唯一索引
        * 主索引
        * 外键索引
        * 复合索引
  * 优化方式
    * 失效条件
    * 底层结构
  * sql语法
    * join、union、子查询、having、group by
  * 引擎对比
    * InnoDB
    * MyISAM
  * 锁
    * 行锁、表锁、页级锁、意向锁、读锁、写锁、悲观锁、乐观锁、以及枷锁的select sql方式
  * 隔离级别
    * 脏读
    * 不可重复读
    * 幻读
  * 事务的ACID
  * B树
  * B+树
  * 优化
    * explain
    * 慢查询
    * show profile
    * 数据库的范式
    * 分库分表
    * 主从复制
    * 读写分离
### NoSql
  * redis
  * memcached
  * mongdb

### 操作系统
   * 进程通信IPC，与线程区别
   * OS集中策略、进程调度
   * 互斥与死锁
   * linux相关命令
        * cpu命令
        * 内存
        * 部署
        * vim
### java
   * 面向对象
   * [集合](https://earyant.github.io/2017/09/10/java%E9%9B%86%E5%90%88%E6%A1%86%E6%9E%B6/)
        * map
            * [Hashmap](https://earyant.github.io/2017/08/09/HashMap%E5%85%A8%E8%A7%A3%E6%9E%90/)
            * EnumMap: 枚举类型座位键值的Map，效率要高于HashMap
            * HashTable：
            * ConcurrentHashMap：
                * get操作全并发访问，put操作可配置并发操作的哈希表，并发级别可以通过构造参数中的concurrentLevel参数设置(默认级别为16).该参数会在Map内部划分一些分区，在put的时候，只有更新的分区是锁住的。
            * ConcurrentSkipListMap： 基于跳跃表的ConcurrentNavigableMap实现。本质上这种集合可以当做TreeMap的线程安全版本来使用。
            * IdentityHashMap
            * LinkedHashMap
            * TreeMap
            * WeakHashMap
        * list
            * ArrayList
                * 内部用一个整形数字或者数组存储了集合的大小。
                * 访问速度稳定；
                * 在尾部添加成本低，在头部添加成本高(线性复杂度)。
            * LinkedList
                * Deque实现，每一个节点都保存着上一个节点和下一个节点的指针。所以数据的存取和更新都具有线性复杂度。
                [ArrayList和LinkedList对比](https://earyant.github.io/2017/09/10/ArrayList%E5%92%8CLinkedList%E5%AF%B9%E6%AF%94/)
            * Vector
        * set
            * HashSet
            * EnumSet
            * BitSet
            * LinkedHashMap
            * TreeSet
            * ConcurrentSkipListSet ： 使用ConcurrentSkipListMap来存储线程安全的Set。
            * CopyOnWriteArraySet： 使用CopyOnWriteArrayList存储的线程安全的Set

        * Queues/Deques
            * ArrayDeQue ： Deque是基于有首尾指针的数组（环形缓冲区）实现的。和LinkedList不同，这个类没有实现List接口，因此，如果没有收尾元素的话，就不能去除任何元素。比LinkedList好一点，产生的垃圾数量较少。
            * Stack ：后进先出的队列
            -- 并发 --
            * ArrayBlockingQueue： 基于书实现的一个有界阻塞对，大小不能重新定义，试图向一个满的队列添加元素的时候，就会受到阻塞，知道另一个方法从队列中取出元素。
            * ConcurrentLinkedDeque、ConcurrentLinkedQueue：基于链表实现的无解队列，添加元素不阻塞。要求消费者的速度至少要比生产一样快，不然内存就会耗尽，严重依赖于CAS操作。
            * DelayQueue ： 无界的保存Delayed元素的集合，元素只有在延时已经过期的时候才能被取出。队列的第一个元素延期最小(包含负值，延时已经过期)，要实现一个延期任务的队列的时候使用(不要自己手动实现--使用ScheduledThreadPoolExecutor)
            * LinkedBlockingDeque/LinkedBlockingQueue： 可选择有界或者无界基于链表的实现。在队列为空或者满的情况下使用ReentrantLock
            * LinkedTransferQueue: 基于链表的无界队列，除了通常的队列操作，还有一系列的transfer方法，可以让生产者直接给等待的消费者传递信息，这样就不用将元素存储到队列中了，这是一个基于CAS的无锁集合
            * PriorityBlockingQueue：PriorityQueue的无界的版本。
            * SynchronousQueue：一个有界队列，其中没有任何内存容量。这就意味着任何插入操作必须等到响应的取出操作才能执行，反之亦反。如果不需要Queue接口的话，通过Exchanger类也能完成响应的功能。
        * Lists类
            * CopyOnWriteArrayList ： list的实现每一次都会产生一个新的隐含数组副本，所以这个操作成本很高，适合遍历操作比更新操作多的集合，例如listeners/observers集合
        * Collections 工具类
            * checked* : 检查要添加的元素的类型并返回结果，尝试添加非法类型的变量都会抛出一个ClassCastException
                * checkedCollection
                * checkedList
                * checkedMap
                * checkSet
                * checkedSortedMap
                * checkedSortedSet
            * empty* : 返回一个固定的空集合。
                * emptyList
                * emptyMap
                * emptySet
            * singleton* : 返回一个只有一个入口的set、list、map集合
                * singletonList
                * singletonMap
            * synchronized* ： 获得集合的线程安全版本
                * synchronizedCollection
                * synchronizedList
                * synchronizedMap
                * synchronizedSet
                * synchronizedSortedMap
                * synchronizedSortedSet
            * unmodifiable* : 返回一个不可变的集合
                * unmodifiableCollection
                * unmodifiableList
                * unmodifiableMap
                * unmodifiableSet
                * unmodifiableSortedMap
                * unmodifiableSortedSet
            * addAll : 添加一些元素或者一个数组的内容到集合中
            * binarySearch ： 和数组的Arrays.binarySearch功能相同
            * disjoint : 检查两个集合是不是没有相同元素
            * fill ： 用一个指定的值代替集合中的所有元素
            * frequency: 集合中有多少元素是和给定元素相同的。
            * indexOfSubList、lastIndexOfSubList\indexOf\lashIndexOf 找出给定list中第一个出现和最后一个出现的子表
            * max、min 找出基于自然顺序或者比较器排序的集合、最大或者最小的元素
            * replaceAll： 替换所有
            * reverse: 掉到排序元素和集合中的顺序
            * rotate ： 根据给定的距离旋转元素
            * shuffle： 随机排放List集合中的节点，而已给定自己的生成器
            * sort ： 自然排序或者指定的排序器排序
            * swap 交换集合中的两个元素的位置
        * Arrays
            * Arrays.asList(): 可以将Array转换成List。
            * Arrays.binarySearch : 在一个已排序的或者其中一段中快速查找
            * Arrays.copyOf : 如果扩大数组容量又不想改变内容时候用这个方法
            * Arrays.copyOfRange :  可以复制整个数组或者其中一个部分
            * Arrays.deepEquals、Arrays.deepHashCode : Arrays.equals、hashCode的高级版本，支持子数据操作。
            * Arrays.equals : 比较两个数组是否想相等。
            * Arrays.fill  : 用一个给定的值填充整个数组或者其中一个部分
            * Arrays.hashCode : 根据数组内容计算器hash值。
            * Arrays.sort : 对整个数组或者数组一部分进行排序。
            * Arrays.toString : 打印数组的内容
            * 所有集合都可以用 T[] Collection.toArray(T[] a)方法复制到整个数组：
                return coll.toArray(new T[coll.size()]);


   * 并发和多线程
        * 线程池
        * SYNC
        * Lock机制
        * 线程通信
        * volatile
        * ThreadLocal
        * CyclicBarrier
        * Atomic包
        * concurrent包
        * CountDownlatch
        * AQS
        * CAS
   * JVM
        * 内存模型
        * GC垃圾回收机制
        * 分代算法，GC算法
        * 收集器
        * 类加载、双亲委派
        * JVM调优
            * jvm参数
        * 内存泄露和内存溢出
   * IO和NIO
   * 反射和代理、异常、java8、序列化
   * 设计模式：
        * 24中常用的设计模式
        * java源码中或者spring中用到哪些
   * web
        * servlet
        * cookie、session
        * spring
            * aop
            * ioc
            * mvc
            * 事务
            * 动态代理
        * mybatis
        * tomcat
            * 类加载机制
        * hibernate
   * 源码
   * Guava
### 脚本语言
   * python
   * shell


## 架构
  ### 分布式     
   * CAP原理和BASE理论
   * NoSql和KV存储(redis,hbase,mongodb,etcd,springcloud)
   * 负载均衡(原理，cdn，一致性hash)
   * RPC框架
        * 通信 netty
        * 序列化协议 thrift，protobuff
   * 消息队列
        * 原理
        * kafka
        * activeMQ
        * rocketMQ
   * 分布式存储系统
        * GFS
        * HDFS
        * fastDFS
        * 存储模型
            * skipList
            * LSM
   * 分布式事务
        * redis分布锁
   * 分布式锁

   ### 大数据
   * hadoop生态圈
        * hive
        * hbase
        * hdfs
        * zoopkeeper
        * storm
        * kafka
   * spark
   * 搜索引擎与技术
   * 机器学习与技术
   * 人工智能

## 书单
   * 算法与数据结构
        * 数据结构（严蔚敏）/大话数据结构              * 剑指Offer/程序员面试金典/编程珠玑/编程之美/牛客网+leetcode
        * 程序员笔试面试最优解（左程云）/不如直接看左神的笔试面试指南视频
        * 数据结构与算法经典问题解析（Java语言描述）
        * 图解数据结构（使用Java）
   * 计算机网络：
        * 计算机网络（谢希仁）
        * TCP/IP 详解
        * HTTP权威指南
        * 图解TCP/IP
        * 图解HTTP
   * 数据库：//数据库主要是多用，书上主要看索引和性能的部分
        * 高性能MySQL/深入浅出MySQL
   * 操作系统：
        * OS原理：操作系统（课本，黑色的那个）
        * Linux：
            * Linux私房菜 //鸟哥写的，很全，包括bash部分
            * 跟阿铭学Linux //主要偏重于命令和操作，比较浅显
   * java：
        * Java疯狂讲义/Java编程思想/Java核心技术 卷1
        * 深入理解Java虚拟机
        * 并发编程的艺术/多线程编程核心技术
        * Effective Java
        * Java程序员面试笔试宝典 //何昊的那本，个人感觉是突击知识点的神器
        * Java程序性能优化
        * 实战Java高并发程序设计
   * Java Web：
        * Spring实战/轻量级JavaEE 企业应用（红皮，讲SSH的） //主要看最后一部分Spring的就可以
        * 深入JavaWeb技术内幕（阿里 许令波）//这个讲的还是比较深的
        * SpringBoot实战/深入实践SpringBoot
   * 设计模式：
        * 大话设计模式 //通俗易懂
        * 各类博客的总结
   * 分布式与大数据：
        * 分布式服务框架原理与实践
        * 大型网站技术架构
        * Hadoop实战（hadoop体系包括得很全）
   * 其他：
        * Git：
            * Git权威指南
            * Git官方讲解视频（牛客网有带字幕的）
        * Redis：
            * Redis实战
   * docker
   * springcloud




感谢：
[这可能不只是一篇面经](https://www.nowcoder.com/discuss/29890?hmsr=toutiao.io&source=rss&utm_medium=toutiao.io&utm_source=toutiao.io)
