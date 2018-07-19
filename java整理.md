---
title: java整理
date: 2017-09-09 21:36:59
tags:
top: true
---

## 算法和数据结构（性能，场景）：

### 数据结构：

-   数组
-   链表
-   树
    -   二叉树
        -   满二叉树
        -   完美二叉树
        -   完全二叉树
        -   二叉搜索树： 其任何节点中的值都会大于或者等于其做字数中存储的值并小于或者等于其右子树的值。
            -   时间复杂度：
                -   索引: O(log(n))
                -   搜索: O(log(n))
                -   插入: O(log(n))
                -   删除: O(log(n))
    -   红黑树
    -   AVL树
    -   Hash树
    -   tire树
    -   b-树（读b树，也叫balance树，不读b减树 “-”代表横杠）
    -   b+树
    -   LSM（Log-Structured Merge-Trees）树
    -   Trie 字典树，基数树或者前缀树，能够存储键为字符串的动态集合或者关联数组的搜索树，树中节点没有存储键值，而是在该节点在树中的挂载位置决定了其关联键值，某个节点的所有子节点都拥有相同的前缀，整树根节点则是空字符串
    -   Fenwick Tree：树状数组，又称为 Binary Indexed Tree，其表现形式为树，不过本质上是以数组实现，数组的下标代表着树中的顶点，每个顶点的父节点或者子节点的下标能够通过位运算获得，数组中的每个元素包含了预计算的区间值之和，在整颗数更新的过程中同样会更新这些预计算的值。
        -   时间复杂度：
            -   区间求值： O(log(n))
            -   更新： O(log(n))
    -   Segment Tree
        -   线段树是用于存放间隔或者线段的树形数据结构，它允许快速查找某一个节点在某若干条线段中出现的次数。
            -   区间查询： O(log(n))
            -   更新: O(log(n))
-   队列
-   栈
-   堆
     堆是一种特殊的基于树的满足某些特性的数据结构，整个堆中的所有父子节点的键值都会满足相同的排序条件。堆更准确地可以分为最大堆与最小堆，在最大堆中，父节点的键值永远大于或者等于子节点的值，并且整个堆中的最大值存储于根节点；而最小堆中，父节点的键值永远小于或者等于其子节点的键值，并且整个堆中的最小值存储于根节点。
-   Hashing
    -   哈希能够将任意长度的数据映射到固定长度的数据，哈希函数返回的即是哈希值，如果两个不同的键得到相同的哈希值，将这种现象称为碰撞。
    -   Hash Map
         是一种能够建立起键与值之间关系的数据结构，Hash Map能够使用哈希函数将键转化为桶或者槽中的下标，从而优化对于目标值的搜索速度。
    -   碰撞解决：
        -   链地址法：
             每个桶都是互相独立的，包含一系列索引的列表，搜索操作的时间复杂度即是搜索桶的时间（固定时间）与遍历列表的时间之和。
        -   开地址法：
             当插入新值时，会判断该值对应的哈希桶是否存在，如果存在则根据某种算法一次选择下一个可能的位置，知道找到一个尚未被占用的地址，所谓开地址法也是只某个元素的位置并不永远由其哈希值决定。
-   Graph
             图是一种数据元素间为多对多关系的数据结构，加上一组基本操作构成的抽象数据类型
             \- 无向图：
                无向图具有对称的邻接矩阵，因此如果存在某条从节点u到节点v的边，反之从v到u的边也存在。
             \- 有向图：
                有向图的邻接矩阵是非对称的，即如果存在从u到v的表并不意味着存在v到u的边。
    ### [算法](https://blog.csdn.net/gane-cheng/article/details/52652705)：
-   查找：

    -   二分查找，以及变种二分查找。
    -   [深度优先、广度优先](https://www.cnblogs.com/0kk470/p/7555033.html)
    -   [贪心算法](https://www.cnblogs.com/MrSaver/p/8641971.html)
        -   [参考二](https://blog.csdn.net/a345017062/article/details/52443781)
    -   [回溯算法](https://blog.csdn.net/qfikh/article/details/51960331)
    -   [剪枝算法](https://blog.csdn.net/luningcsdn/article/details/50930276)
    -   [动态规划](https://www.cnblogs.com/little-YTMM/p/5372680.html)
        -   [参考二](https://blog.csdn.net/yao_zi_jie/article/details/54580283)
    -   [朴素贝叶斯](https://blog.csdn.net/amds123/article/details/70173402)
        -   [参考二](http://www.ruanyifeng.com/blog/2011/08/bayesian_inference_part_one.html)
        -   [参考三](http://www.ruanyifeng.com/blog/2011/08/bayesian_inference_part_two.html)
    -   [推荐算法](http://www.infoq.com/cn/articles/recommendation-algorithm-overview-part01)
    -   [参考二](https://www.oschina.net/news/51297/top-10-open-source-recommendation-systems)
    -   [最小生成树算法](https://blog.csdn.net/luoshixian099/article/details/51908175)
    -   [最短路径算法](https://blog.csdn.net/qq_35644234/article/details/60870719)

-   排序：
    -   7种排序
            \- [选择排序](https://www.cnblogs.com/shen-hua/p/5424059.html)
              > 每一趟从待排序的记录中选出最小的元素，顺序放在已排好序的序列最后，直到全部记录排序完毕。
            - [冒泡排序](https://blog.csdn.net/shuaizai88/article/details/73250615)
                > 相邻元素前后交换、把最大的排到最后。
                > 时间复杂度 O(n²)
            - [插入排序](https://www.cnblogs.com/hapjin/p/5517667.html)
            -   [快速排序](http://developer.51cto.com/art/201403/430986.htm)
                -   稳定： 否
                -   时间复杂度：
                    -   最优时间：O(nlog(n))
                    -   最坏时间: O(n^2)
                    -   平均时间：O(nlog(n))
            -   [归并排序：](http://www.cnblogs.com/chengxiao/p/6194356.html)
                -   分治算法，不断将某个数组分为两个部分，分别对左子数组与右子数组进行排序，然后将两个数组合并为新的有序数组
                -   稳定：是
                -   时间复杂度
                    -   最优时间： O(nlog(n))
                    -   最坏时间： O(nlog(n))
                    -   平均时间： O(nlog(n))
            -   [桶排序：](http://blog.51cto.com/ahalei/1362789)
                - [参考二](https://blog.csdn.net/sunjinshengli/article/details/70738527)
                -   桶排序将数组分到有限数量的筒子里，每个桶子再个别排序，有可能在使用别的排序算法或者以地柜方式继续使用桶排序。
                -   时间复杂度：
                    -   最优时间： O(n+k)
                    -   最坏时间： O(n^2)
                    -   平均时间： O(n+k)
                        [参考](https://github.com/kdn251/interviews/blob/master/README-zh-cn.md)
            -  [计数排序](https://www.cnblogs.com/suvllian/p/5495780.html)
            -   时间、空间复杂度（理解并分析）
            -   动态规划、贪心算法
        ### 计算机网络
-   OSI7层协议（TCP四层）
    -   URL到页面的过程
-   http&#x3A;
    -   http/https 1.0、1.1、2.0
    -   get/post 以及幂等性
    -   http相关头协议
-   网络攻击（CSRF、XSS）
-   TCP/IP
    -   三次握手、四次握手
    -   拥塞控制（过程、阙值）
    -   TCP与UDP比较
    -   子网掩码
    -   DDos攻击
-   BIO、IO、NIO、AIO
    -   原理
    -   Netty
    -   linux内核的select poll epoll

### 数据库(mysql)

-   索引
    -   分类
        -   全文索引
        -   HASH索引
        -   BTree索引
        -   RTree索引
        -   普通索引
        -   唯一索引
        -   主索引
        -   外键索引
        -   复合索引
-   优化方式
    -   失效条件
    -   底层结构
-   sql语法
    -   join、union、子查询、having、group by
-   引擎对比
    -   InnoDB
    -   MyISAM
-   [锁]{}
    -   行锁、表锁、页级锁、意向锁、读锁、写锁、悲观锁、乐观锁、以及枷锁的select sql方式
-   隔离级别
    -   脏读
    -   不可重复读
    -   幻读
-   事务的ACID
-   B树
-   B+树
-   优化
    -   explain
    -   慢查询
    -   show profile
    -   数据库的范式
    -   分库分表
    -   主从复制
    -   读写分离

### NoSql

-   redis
-   memcached
-   mongdb
-   [《Memcached 教程》](http://www.runoob.com/Memcached/Memcached-tutorial.html)
-   [《深入理解Memcached原理》](https://blog.csdn.net/chenleixing/article/details/47035453)
-   [《Memcached软件工作原理》](https://www.jianshu.com/p/36e5cd400580)
-   [《Memcache技术分享：介绍、使用、存储、算法、优化、命中率》](http://zhihuzeye.com/archives/2361)
-   [《memcache 中 add 、 set 、replace 的区别》](https://blog.csdn.net/liu251890347/article/details/37690045)
-   [《memcached全面剖析》](https://pan.baidu.com/s/1qX00Lti?errno=0&errmsg=Auth%20Login%20Sucess&&bduss=&ssnerror=0&traceid=)

### 设计模式

-   [设计模式的六大原则](https://blog.csdn.net/q291611265/article/details/48465113)
    -   开闭原则：对扩展开放,对修改关闭，多使用抽象类和接口。
    -   里氏替换原则：基类可以被子类替换，使用抽象类继承,不使用具体类继承。
    -   依赖倒转原则：要依赖于抽象,不要依赖于具体，针对接口编程,不针对实现编程。
    -   接口隔离原则：使用多个隔离的接口,比使用单个接口好，建立最小的接口。
    -   迪米特法则：一个软件实体应当尽可能少地与其他实体发生相互作用，通过中间类建立联系。
    -   合成复用原则：尽量使用合成/聚合,而不是使用继承。
-   [23种常见设计模式](http://www.runoob.com/design-pattern/design-pattern-tutorial.html)
    -   [参考二](https://www.cnblogs.com/susanws/p/5510229.html)
-   [《细数JDK里的设计模式》](http://blog.jobbole.com/62314/)

    -   结构型模式：

        -   适配器：用来把一个接口转化成另一个接口，如 java.util.Arrays#asList()。
        -   桥接模式：这个模式将抽象和抽象操作的实现进行了解耦，这样使得抽象和实现可以独立地变化，如JDBC；
        -   组合模式：使得客户端看来单个对象和对象的组合是同等的。换句话说，某个类型的方法同时也接受自身类型作为参数，如 Map.putAll，List.addAll、Set.addAll。
        -   装饰者模式：动态的给一个对象附加额外的功能，这也是子类的一种替代方式，如 java.util.Collections#checkedList|Map|Set|SortedSet|SortedMap。
        -   享元模式：使用缓存来加速大量小对象的访问时间，如 valueOf(int)。
        -   代理模式：代理模式是用一个简单的对象来代替一个复杂的或者创建耗时的对象，如 java.lang.reflect.Proxy

    -   创建模式:

        -   抽象工厂模式：抽象工厂模式提供了一个协议来生成一系列的相关或者独立的对象，而不用指定具体对象的类型，如 java.util.Calendar#getInstance()。
        -   建造模式(Builder)：定义了一个新的类来构建另一个类的实例，以简化复杂对象的创建，如：java.lang.StringBuilder#append()。
        -   工厂方法：就是 一个返\* 回具体对象的方法，而不是多个，如 java.lang.Object#toString()、java.lang.Class#newInstance()。
        -   原型模式：使得类的实例能够生成自身的拷贝、如：java.lang.Object#clone()。
        -   单例模式：全局只有一个实例，如 java.lang.Runtime#getRuntime()。

    -   行为模式：

        -   责任链模式：通过把请求从一个对象传递到链条中下一个对象的方式，直到请求被处理完毕，以实现对象间的解耦。如 javax.servlet.Filter#doFilter()。
        -   命令模式：将操作封装到对象内，以便存储，传递和返回，如：java.lang.Runnable。
        -   解释器模式：定义了一个语言的语法，然后解析相应语法的语句，如，java.text.Format，java.text.Normalizer。
        -   迭代器模式：提供一个一致的方法来顺序访问集合中的对象，如 java.util.Iterator。
        -   中介者模式：通过使用一个中间对象来进行消息分发以及减少类之间的直接依赖，java.lang.reflect.Method#invoke()。
        -   空对象模式：如 java.util.Collections#emptyList()。
        -   观察者模式：它使得一个对象可以灵活的将消息发送给感兴趣的对象，如 java.util.EventListener。
        -   模板方法模式：让子类可以重写方法的一部分，而不是整个重写，如 java.util.Collections#sort()。

-   [《Spring-涉及到的设计模式汇总》](https://www.cnblogs.com/hwaggLee/p/4510687.html)
-   [《Mybatis使用的设计模式》](https://blog.csdn.net/u012387062/article/details/54719114)

### 操作系统

-   进程通信IPC，与线程区别
-   OS集中策略、进程调度
-   互斥与死锁
-   linux相关命令
    -   cpu命令
    -   内存
    -   部署
    -   vim
        ### java
-   面向对象
-   [集合](https://earyant.github.io/2017/09/10/java%E9%9B%86%E5%90%88%E6%A1%86%E6%9E%B6/)

    -   map
        -   [Hashmap](https://earyant.github.io/2017/08/09/HashMap%E5%85%A8%E8%A7%A3%E6%9E%90/)
        -   EnumMap: 枚举类型座位键值的Map，效率要高于HashMap
        -   HashTable：
        -   ConcurrentHashMap：
            -   get操作全并发访问，put操作可配置并发操作的哈希表，并发级别可以通过构造参数中的concurrentLevel参数设置(默认级别为16).该参数会在Map内部划分一些分区，在put的时候，只有更新的分区是锁住的。
        -   ConcurrentSkipListMap： 基于跳跃表的ConcurrentNavigableMap实现。本质上这种集合可以当做TreeMap的线程安全版本来使用。
        -   IdentityHashMap
        -   LinkedHashMap
        -   TreeMap
        -   WeakHashMap
    -   list
        -   ArrayList
            -   内部用一个整形数字或者数组存储了集合的大小。
            -   访问速度稳定；
            -   在尾部添加成本低，在头部添加成本高(线性复杂度)。
        -   LinkedList
            -   Deque实现，每一个节点都保存着上一个节点和下一个节点的指针。所以数据的存取和更新都具有线性复杂度。
                [ArrayList和LinkedList对比](https://earyant.github.io/2017/09/10/ArrayList%E5%92%8CLinkedList%E5%AF%B9%E6%AF%94/)
            -   时间复杂度：
                -   索引:  O(n)
                -   搜索:  O(n)
                -   插入:  O(1)
                -   移除:  O(1)
        -   Vector
    -   set

        -   HashSet
        -   EnumSet
        -   BitSet
        -   LinkedHashMap
        -   TreeSet
        -   ConcurrentSkipListSet ： 使用ConcurrentSkipListMap来存储线程安全的Set。
        -   CopyOnWriteArraySet： 使用CopyOnWriteArrayList存储的线程安全的Set

    -   Queues/Deques

        -   ArrayDeQue ： Deque是基于有首尾指针的数组（环形缓冲区）实现的。和LinkedList不同，这个类没有实现List接口，因此，如果没有收尾元素的话，就不能去除任何元素。比LinkedList好一点，产生的垃圾数量较少。
        -   [Queue](https://www.cnblogs.com/lemon-flm/p/7877898.html)
            -   enqueue： 插入到队列中
            -   dequeue： 将队头移除
            -   时间复杂度：
                -   索引: O(n)
                -   搜索: O(n)
                -   插入: O(1)
                -   移除: O(1)
        -   Stack ：后进先出的队列

            -   push： 将元素压入栈
            -   pop：  将栈顶元素移除
            -   时间复杂度：
                -   索引: O(n)
                -   搜索: O(n)
                -   插入: O(1)
                -   移除: O(1)

            \-- 并发 --

        -   ArrayBlockingQueue： 基于书实现的一个有界阻塞对，大小不能重新定义，试图向一个满的队列添加元素的时候，就会受到阻塞，知道另一个方法从队列中取出元素。
        -   ConcurrentLinkedDeque、ConcurrentLinkedQueue：基于链表实现的无解队列，添加元素不阻塞。要求消费者的速度至少要比生产一样快，不然内存就会耗尽，严重依赖于CAS操作。
        -   DelayQueue ： 无界的保存Delayed元素的集合，元素只有在延时已经过期的时候才能被取出。队列的第一个元素延期最小(包含负值，延时已经过期)，要实现一个延期任务的队列的时候使用(不要自己手动实现--使用ScheduledThreadPoolExecutor)
        -   LinkedBlockingDeque/LinkedBlockingQueue： 可选择有界或者无界基于链表的实现。在队列为空或者满的情况下使用ReentrantLock
        -   LinkedTransferQueue: 基于链表的无界队列，除了通常的队列操作，还有一系列的transfer方法，可以让生产者直接给等待的消费者传递信息，这样就不用将元素存储到队列中了，这是一个基于CAS的无锁集合
        -   PriorityBlockingQueue：PriorityQueue的无界的版本。
        -   SynchronousQueue：一个有界队列，其中没有任何内存容量。这就意味着任何插入操作必须等到响应的取出操作才能执行，反之亦反。如果不需要Queue接口的话，通过Exchanger类也能完成响应的功能。

    -   Lists类
        -   CopyOnWriteArrayList ： list的实现每一次都会产生一个新的隐含数组副本，所以这个操作成本很高，适合遍历操作比更新操作多的集合，例如listeners/observers集合
    -   Collections 工具类
        -   checked\* : 检查要添加的元素的类型并返回结果，尝试添加非法类型的变量都会抛出一个ClassCastException
            -   checkedCollection
            -   checkedList
            -   checkedMap
            -   checkSet
            -   checkedSortedMap
            -   checkedSortedSet
        -   empty\* : 返回一个固定的空集合。
            -   emptyList
            -   emptyMap
            -   emptySet
        -   singleton\* : 返回一个只有一个入口的set、list、map集合
            -   singletonList
            -   singletonMap
        -   synchronized\* ： 获得集合的线程安全版本
            -   synchronizedCollection
            -   synchronizedList
            -   synchronizedMap
            -   synchronizedSet
            -   synchronizedSortedMap
            -   synchronizedSortedSet
        -   unmodifiable\* : 返回一个不可变的集合
            -   unmodifiableCollection
            -   unmodifiableList
            -   unmodifiableMap
            -   unmodifiableSet
            -   unmodifiableSortedMap
            -   unmodifiableSortedSet
        -   addAll : 添加一些元素或者一个数组的内容到集合中
        -   binarySearch ： 和数组的Arrays.binarySearch功能相同
        -   disjoint : 检查两个集合是不是没有相同元素
        -   fill ： 用一个指定的值代替集合中的所有元素
        -   frequency: 集合中有多少元素是和给定元素相同的。
        -   indexOfSubList、lastIndexOfSubList\\indexOf\\lashIndexOf 找出给定list中第一个出现和最后一个出现的子表
        -   max、min 找出基于自然顺序或者比较器排序的集合、最大或者最小的元素
        -   replaceAll： 替换所有
        -   reverse: 掉到排序元素和集合中的顺序
        -   rotate ： 根据给定的距离旋转元素
        -   shuffle： 随机排放List集合中的节点，而已给定自己的生成器
        -   sort ： 自然排序或者指定的排序器排序
        -   swap 交换集合中的两个元素的位置
    -   Arrays
        -   Arrays.asList(): 可以将Array转换成List。
        -   Arrays.binarySearch : 在一个已排序的或者其中一段中快速查找
        -   Arrays.copyOf : 如果扩大数组容量又不想改变内容时候用这个方法
        -   Arrays.copyOfRange :  可以复制整个数组或者其中一个部分
        -   Arrays.deepEquals、Arrays.deepHashCode : Arrays.equals、hashCode的高级版本，支持子数据操作。
        -   Arrays.equals : 比较两个数组是否想相等。
        -   Arrays.fill  : 用一个给定的值填充整个数组或者其中一个部分
        -   Arrays.hashCode : 根据数组内容计算器hash值。
        -   Arrays.sort : 对整个数组或者数组一部分进行排序。
        -   Arrays.toString : 打印数组的内容
        -   所有集合都可以用 T\[] Collection.toArray(T\[] a)方法复制到整个数组：
              return coll.toArray(new T[coll.size()]);


-   并发和多线程

    -   参考：
        -   [Java 并发知识合集](https://github.com/CL0610/Java-concurrency)
        -   [JAVA并发知识图谱](https://github.com/CL0610/Java-concurrency/blob/master/Java%E5%B9%B6%E5%8F%91%E7%9F%A5%E8%AF%86%E5%9B%BE%E8%B0%B1.png)
        -   java并发编程网
        -   [《40个Java多线程问题总结》](http://www.importnew.com/18459.html)
        -   [《Java并发编程——线程安全及解决机制简介》](https://www.cnblogs.com/zhanht/p/5450325.html)
    -   锁
        -   ReetrantLock
        -   ReetranReadWriteLock
        -   Condition
    -   开发工具类

        -   CyclicBarrier
        -   CountDownLatch
        -   Semphere

    -   并发集合
        -   ConcurrentHashMap
        -   ConcurrentLinkedQueue
    -   线程池
        -   Excutor
        -   ThreadPoolExcutor
        -   Callable和Future
        -   ScheduledExecutorService
    -   原子操作
        -   基本类型
            -   AtomicBoolean
            -   AtomicInteger
            -   AtomicLong
        -   引用类型
            -   AtomicReference
            -   AtomicReferenceArrayFieldUpdater
        -   数组
            -   AtomicIntegerArray
            -   AtomicLongArray
            -   AtomicReferenceArray
    -   synchronized
        -   同步、重量级锁，synchronized原理
        -   锁优化
            -   自旋锁
            -   轻量级锁
            -   重量级锁
            -   偏向锁
    -   Lock机制
    -   线程通信
    -   volatile
        -   实现机制
        -   内存语义
        -   内存模型
    -   ThreadLocal
    -   Fork\\Join
    -   concurrent包
    -   AQS
        -   AbstractqueuedSynchronized同步器
        -   CLH同步队列
        -   同步状态的获取及释放
        -   线程阻塞和唤醒
    -   CAS
        -   Compare And Swap缺陷

-   优化调优
    -   工具
        -   MAT
        -   Jmeter
    -   理解性能优化
        -   性能基准
        -   性能优化到底是什么
        -   衡量维度
    -   JVM调优
        -   什么是JVM内存模型JMM
        -   各垃圾回收气使用场景
        -   理解GC日志
        -   实战MAT分析DUMP文件
    -   Tomcat调优
        -   tomcat运行机制
        -   线程模型
        -   系统参数认识及调优
        -   基准测试
    -   MySQL调优
        -   理解MySQL底层B+树
        -   SQL执行计划详解索引优化详解
        -   SQL语句优化
-   JVM

    > 《深入理解Java虚拟机：JVM高级特性与最佳实践》

    -   内存模型
        -   重排序
        -   顺序一致性
        -   happens-hefore
        -   as-if-serial
    -   GC垃圾回收机制
    -   分代算法，GC算法
    -   收集器
    -   类加载、双亲委派
    -   JVM调优
        -   jvm参数
    -   内存泄露和内存溢出

-   IO和NIO
-   反射和代理、异常、java8、序列化
-   设计模式：
    -   24中常用的设计模式
        -   proxy代理模式
        -   Factory工厂模式
        -   Singleton单例模式
        -   Delegate委派模式
        -   Strategy策略模式
        -   prototype原型模式
        -   Adapter适配器模式
    -   java源码中或者spring中用到哪些
-   web
    -   servlet
    -   cookie、session
    -   spring
        -   aop
            -   AOP设计原理
        -   ioc
            -   IOC容器设计原理及高级特性
        -   FactoryBean与BeanFactory
        -   mvc
        -   [事务](https://earyant.github.io/2017/09/17/Spring%E4%B8%AD%E7%9A%84%E4%BA%8B%E5%8A%A1%E7%AE%A1%E7%90%86/)
        -   基于SpringJDBC手写ORM框架
        -   SpringMVC九大组件
        -   手写实现SpringMVC框架
        -   SpringMVC与Struts2对比分析
        -   Spring5新特性
        -   动态代理
    -   mybatis
        -   代码自动生成器
        -   关联查询、嵌套 查询
        -   缓存使用场景及选择策略
        -   Spring集成下的SqlSession与Mapper
        -   事务
        -   分析MyBatis的动态代理
        -   手写实现Mini版的MyBatis
    -   tomcat
        -   参考
            -   [《TOMCAT原理详解及请求过程》](https://www.cnblogs.com/hggen/p/6264475.html)
            -   [《Tomcat服务器原理详解》](https://www.cnblogs.com/crazylqy/p/4706223.html)
            -   [《Tomcat 系统架构与设计模式,第 1 部分: 工作原理》](https://www.ibm.com/developerworks/cn/java/j-lo-tomcat1/)
            -   [《四张图带你了解Tomcat系统架构》](https://blog.csdn.net/xlgen157387/article/details/79006434)
            -   [《JBoss vs. Tomcat: Choosing A Java Application Server》](https://www.futurehosting.com/blog/jboss-vs-tomcat-choosing-a-java-application-server/)
                -   Tomcat 是轻量级的 Serverlet 容器，没有实现全部 JEE 特性（比如持久化和事务处理），但可以通过其他组件代替，比如Srping。
                -   Jboss 实现全部了JEE特性，软件开源免费、文档收费。
                -   [《Tomcat 调优方案》](https://www.cnblogs.com/sunfenqing/p/7339058.html)
                -   启动NIO模式（或者APR）；调整线程池；禁用AJP连接器（Nginx+tomcat的架构，不需要AJP）；
                -   [《tomcat http协议与ajp协议》](http://blog.chinaunix.net/uid-20662363-id-3012760.html)
                -   [《AJP与HTTP比较和分析》](http://dmouse.iteye.com/blog/1354527)
                    -   AJP 协议（8009端口）用于降低和前端Server（如Apache，而且需要支持AJP协议）的连接数(前端)，通过长连接提高性能。
                    -   并发高时，AJP协议优于HTTP协议。
        -   类加载机制
    -   Jetty
        -   参考
            -   [《Jetty 的工作原理以及与 Tomcat 的比较》](https://www.ibm.com/developerworks/cn/java/j-lo-jetty/)
            -   [《jetty和tomcat优势比较》](https://blog.csdn.net/doutao6677/article/details/51957288)
    -   hibernate
-   源码
-   Guava

### 脚本语言

-   python
-   shell

## 架构

### 微服务

-   微框架
    -   与微服务之间的关系
    -   热部署实战
    -   核心组件Starter\\Actuator\\AutoConfiguration\\Cli
    -   集成Mybatis实现多数据源路由实战
    -   集成Dubbo实战
    -   集成Redis缓存实战
    -   集成Swagger2实战
    -   API管理及测试体系
    -   实现多环境配置动态解析
-   SpringCloud
    -   注册中心
        -   Eureka
        -   Zookeeper
    -   Ribbon集成REST实现负载均衡
    -   Fegion生命是服务调用
    -   Hystrix服务熔断降级方式
    -   Zuul实现微服务网管
    -   Config分布式统一配置中心（与disconf作对比）
    -   Sleuth调用链路跟踪
    -   BUS消息总线
    -   基于Hystrix实现接口降级实战
    -   集成SpringCloud实现统一整合方案
-   Docker虚拟化

    -   Docker的镜像、仓库、容器
    -   Docker File文件
    -   Docker Compose
    -   Swarm K8s

-   漫谈微服务架构
        \-  SOA架构和微服务架构之间的区别和联系
        \- 如何设计微服务及设计原则
        \- 全局分析Spring Cloud各个组件所能解决的问题

    ### 分布式

-   分布式架构原理
    -   分布式架构严谨过程
    -   如何把应用从单机扩展到分布式
    -   CDN加速静态文件访问
    -   系统监控、容灾、存储动态扩容
    -   架构设计及业务驱动划分
    -   CAP原理和BASE理论
-   分布式架构策略
    -   分布式架构网络通信原理剖析
    -   通信协议中的序列化和反序列化
    -   基于框架的RPC技术
        -   WebService、RMI、Hession
    -   深入分析Zookeeper在disconf配置中心的应用
    -   基于Zookeeper实现分布式服务器动态上下线感知
    -   深入分析Zookeeper Zab协议及选举机制源码解读
    -   Dubbo管理中心及监控平台安装部署
    -   基于Dubbo的分布式系统架构实战
    -   Dubbo容错机制及高扩展性分析
-   分布式架构中间件
    -   分布式消息通信
    -   ActiveMq\\Kafka\\RabbitMQ
    -   Redis主从复制原理及无磁盘复制分析
    -   图解Redis中AOF和RDB持久化策略的原理
    -   MongoDb企业级集群解决方案
    -   MongoDb数据分片、转存及恢复策略
    -   基于OpenResty部署应用层Nginx以及Nginx+lua实践
    -   Nginx反向代理服务器及负载均衡服务配置实战
    -   基于Netty实现高性能IM聊天
    -   基于Netty实现Dubbo多协议通信支持
    -   Netty无锁化串行设计及高并发处理机制
-   分布式架构实战
    -   分布式全局ID生成方案
    -   Session跨域共享及企业级单点登录解决方案实战
    -   分布式事务解决方案实战
    -   高并发下的服务降级、限流实战
    -   基于分布式架构下的分布式锁的解决方案实战
    -   分布式架构下实现分布式定时调度
-   NoSql和KV存储(redis,hbase,mongodb,etcd,springcloud)
-   负载均衡(原理，cdn，一致性hash)
-   RPC框架
    -   通信 netty
    -   序列化协议 thrift，protobuff
-   消息队列
    -   原理
    -   kafka
    -   activeMQ
    -   rocketMQ
-   分布式存储系统
    -   GFS
    -   HDFS
    -   fastDFS
    -   存储模型
        -   skipList
        -   LSM
-   分布式事务
    -   redis分布锁
-   分布式锁

    ### 大数据

-   hadoop生态圈
    -   hive
    -   hbase
    -   hdfs
    -   zoopkeeper
    -   storm
    -   kafka
-   spark
-   搜索引擎与技术
-   机器学习与技术
-   人工智能

### 运维 & 统计 & 技术支持

-   常规监控

    -   [《腾讯业务系统监控的修炼之路》](https://blog.csdn.net/enweitech/article/details/77849205)
        -   监控的方式：主动、被动、旁路(比如舆情监控)
        -   监控类型： 基础监控、服务端监控、客户端监控、 监控、用户端监控
        -   监控的目标：全、块、准
        -   核心指标：请求量、成功率、耗时
    -   [《开源还是商用？十大云运维监控工具横评》](https://www.oschina.net/news/67525/monitoring-tools)

        > Zabbix、Nagios、Ganglia、Zenoss、Open-falcon、监控宝、 360网站服务监控、阿里云监控、百度云观测、小蜜蜂网站监测等。

    -   [《监控报警系统搭建及二次开发经验》](http://developer.51cto.com/art/201612/525373.htm)

-   命令行监控工具
    -   [《常用命令行监控工具》](https://coderxing.gitbooks.io/architecture-evolution/di-er-pian-ff1a-feng-kuang-yuan-shi-ren/44-an-quan-yu-yun-wei/445-fu-wu-qi-zhuang-tai-jian-ce/4451-ming-ling-xing-gong-ju.html)
    -   [《20个命令行工具监控 Linux 系统性能》](http://blog.jobbole.com/96846/)
    -   [《JVM性能调优监控工具jps、jstack、jmap、jhat、jstat、hprof使用详解》](https://my.oschina.net/feichexia/blog/196575)
    -   [日志分析常用命令](https://earyant.github.io/2018/05/04/%E6%97%A5%E5%BF%97%E5%88%86%E6%9E%90%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4/)
    -   [日志分析脚本](https://earyant.github.io/2018/05/04/%E6%97%A5%E5%BF%97%E5%88%86%E6%9E%90%E8%84%9A%E6%9C%AC/)
-   统计分析

    -   [《流量统计的基础：埋点》](https://zhuanlan.zhihu.com/p/25195217)
        > 常用指标：访问与访客、停留时长、跳出率、退出率、转化率、参与度
    -   [《APP埋点常用的统计工具、埋点目标和埋点内容》](http://www.25xt.com/company/17066.html)
        > 第三方统计：友盟、百度移动、魔方、App Annie、talking data、神策数据等。
    -   [《美团点评前端无痕埋点实践》](https://tech.meituan.com/mt-mobile-analytics-practice.html)
        > 所谓无痕、即通过可视化工具配置采集节点，在前端自动解析配置并上报埋点数据，而非硬编码。

-   持续集成(CI/CD)
    -   [《持续集成是什么？》](http://www.ruanyifeng.com/blog/2015/09/continuous-integration.html)
    -   [《8个流行的持续集成工具》](https://www.testwo.com/article/1170)
    -   [《使用Jenkins进行持续集成》](https://www.liaoxuefeng.com/article/001463233913442cdb2d1bd1b1b42e3b0b29eb1ba736c5e000)
-   环境分离
    -   [《开发环境、生产环境、测试环境的基本理解和区》](https://my.oschina.net/sancuo/blog/214904)

### 测试

-   TDD 理论
    -   \-[《深度解读 - TDD（测试驱动开发）》](https://www.jianshu.com/p/62f16cd4fef3)
        -   基于测试用例编码功能代码，XP（Extreme Programming）的核心实践.
        -   好处：一次关注一个点，降低思维负担；迎接需求变化或改善代码的设计；提前澄清需求；快速反馈；
-   单元测试
    -   [《Java单元测试之JUnit篇》](https://www.cnblogs.com/happyzm/p/6482886.html)
    -   [《JUnit 4 与 TestNG 对比》](<TestNG 覆盖 JUnit 功能，适用于更复杂的场景。>)
    -   [《单元测试主要的测试功能点》](<模块接口测试、局部数据结构测试、路径测试 、错误处理测试、边界条件测试 。>)
-   压力测试
    -   [《Apache ab 测试使用指南》](https://blog.csdn.net/blueheart20/article/details/52170790)
    -   [《大型网站压力测试及优化方案》](https://www.cnblogs.com/binyue/p/6141088.html)
    -   [《10大主流压力/负载/性能测试工具推荐》](http://news.chinabyte.com/466/14126966.shtml)
    -   [《真实流量压测工具 tcpcopy应用浅析》](http://quentinxxz.iteye.com/blog/2249799)
    -   [《nGrinder 简易使用教程》](https://www.cnblogs.com/jwentest/p/7136727.html)
-   全链路压测
    -   [《京东618：升级全链路压测方案，打造军演机器人ForceBot》](http://www.infoq.com/cn/articles/jd-618-upgrade-full-link-voltage-test-program-forcebot)
    -   [《饿了么全链路压测的探索与实践》](https://zhuanlan.zhihu.com/p/30306892)
    -   [《四大语言，八大框架｜滴滴全链路压测解决之道》](https://zhuanlan.zhihu.com/p/28355759)
    -   [《全链路压测经验》](https://www.jianshu.com/p/27060fd61f72)
-   A/B 、灰度、蓝绿测试
    -   [《技术干货 | AB 测试和灰度发布探索及实践》](https://testerhome.com/topics/11165)
    -   [《nginx 根据IP 进行灰度发布》](http://blog.51cto.com/purplegrape/1403123)
    -   [《蓝绿部署、A/B 测试以及灰度发布》](https://www.v2ex.com/t/344341)

## 书单

-   算法与数据结构
    -   数据结构（严蔚敏）/大话数据结构              \* 剑指Offer/程序员面试金典/编程珠玑/编程之美/牛客网+leetcode
    -   程序员笔试面试最优解（左程云）/不如直接看左神的笔试面试指南视频
    -   数据结构与算法经典问题解析（Java语言描述）
    -   图解数据结构（使用Java）
-   计算机网络：
    -   计算机网络（谢希仁）
    -   TCP/IP 详解
    -   HTTP权威指南
    -   图解TCP/IP
    -   图解HTTP
-   数据库：//数据库主要是多用，书上主要看索引和性能的部分
    -   高性能MySQL/深入浅出MySQL
-   操作系统：
    -   OS原理：操作系统（课本，黑色的那个）
    -   Linux：
        -   Linux私房菜 //鸟哥写的，很全，包括bash部分
        -   跟阿铭学Linux //主要偏重于命令和操作，比较浅显
-   java：
    -   Java疯狂讲义/Java编程思想/Java核心技术 卷1
    -   深入理解Java虚拟机
    -   并发编程的艺术/多线程编程核心技术
    -   Effective Java
    -   Java程序员面试笔试宝典 //何昊的那本，个人感觉是突击知识点的神器
    -   Java程序性能优化
    -   实战Java高并发程序设计
-   Java Web：
    -   Spring实战/轻量级JavaEE 企业应用（红皮，讲SSH的） //主要看最后一部分Spring的就可以
    -   深入JavaWeb技术内幕（阿里 许令波）//这个讲的还是比较深的
    -   SpringBoot实战/深入实践SpringBoot
-   设计模式：
    -   大话设计模式 //通俗易懂
    -   各类博客的总结
-   分布式与大数据：
    -   分布式服务框架原理与实践
    -   大型网站技术架构
    -   Hadoop实战（hadoop体系包括得很全）
-   其他：
    -   Git：
        -   Git权威指南
        -   Git官方讲解视频（牛客网有带字幕的）
    -   Redis：
        -   Redis实战
-   docker
-   springcloud

## 实战

### 实践一个双十一电商项目

-   用户认证
    -   用户注册
    -   SSO单点登录
    -   第三方登陆
    -   UI界面拦截
    -   业务拦截
-   店铺、商品
    -   聚合检索
    -   动静分离
    -   店铺管理
    -   商品管理
-   订单、支付
    -   订单号统一生成规则
    -   下单流程管理
    -   库存管理
    -   购物车
    -   优惠券支付
    -   积分支付
    -   第三方支付
-   数据统计分析
    -   用户行为分析
    -   行业分析
    -   区域分析
-   通知推送
    -   融云推送
    -   消息中间件
    -   用户群聊
    -   点对点聊天
    -   文件断点续传

感谢：
[这可能不只是一篇面经](https://www.nowcoder.com/discuss/29890?hmsr=toutiao.io&source=rss&utm-medium=toutiao.io&utm-source=toutiao.io)
[java整理](https://github.com/kdn251/interviews/blob/master/README-zh-cn.md)
