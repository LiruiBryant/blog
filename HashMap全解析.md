---
title: HashMap全解析
date: 2017-08-09 15:20:26
tags:
---

# 定义
HashMap实现了Map接口，继承自AbstactMap。其中Map接口定义了键映射到值的规则。
    public class HashMap<K,V>
    extends AbstractMap<K,V>
    implements Map<K,V>, Cloneable, Serializable
    ![](../img/java.util.map类图.png)
*注意*
    * HashMap: 它根据键的hashCode值存储数据，大多数情况下可以直接定位到他的值，因此有很乖的访问速度，但是遍历的顺序是不确定的，HashMap最多只允许一条记录为null，允许多条记录的值为null，HashMap不是线程安全的，即任意时刻有多线程同时写HashMap可能会导致数据不一致问题。
    * HashTable: HashTable是遗留类，很多映射的功能和HashMap类似，但是他是继承自Dictionary类，并且是线程安全的，任何时间只有一个线程能写hashTable。
    * ConcurrentHashMap：
    * LinkedHashMap: 是HashMap的一个自雷，保存了插入顺序，在用iterator遍历LinkedHashMap时，先得到的肯定是新插入的，也可以在构造式带参数，按照访问次数进行排序。
    * TreeMap: TreeMap实现了SortMap接口，能够把保存的记录根据键排序，默认是按照键值升序排序，也可以指定排序的比较器，在使用TreeMap时，key必须实现Comparable接口或者在构造TreeMap传入自定义的Comparator，否则会在运行时抛出ClassCastExeption。
# 构造函数
* HashMap()：默认构造器，构造一个初始容量为10和默认加载银子为0.75的空HashMap
* HashMap(int initialCapacity):构造一个指定容量的和默认加载银子为0.75的空HashMap
* HashMap(int initialCapacity, float loadFactor)： 构造一个指定初始容量和加载银子的空HashMap；
  *其中initialCapacity不能小于0，当它大于1 << 30的时候，它就等于1 << 30*
     if (initialCapacity > MAXIMUM_CAPACITY)
            initialCapacity = MAXIMUM_CAPACITY;

初始容量：代表哈希表中通的数量，
加载因子： 代表哈希表在自动增加之前可以达到的尺度。
# 数据结构
列表散列：
![](http://images.cnitblog.com/blog/381060/201401/152128351581.png)
数组+链表+红黑树(jdk8中增加红黑树)
![](../img/hashMap内存结构.png)
HashMap的底层实现还是数组，只不过数组的每一项都是一条链，其中initialCapacity参数代表了该数组额长度。
```java
     /**
     * Returns a power of two size for the given target capacity.
     */
    static final int tableSizeFor(int cap) {
        int n = cap - 1;
        n |= n >>> 1;
        n |= n >>> 2;
        n |= n >>> 4;
        n |= n >>> 8;
        n |= n >>> 16;
        return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
    }
```
这一段表示将初始容量变成向下靠近2的幂次方的数。
# Node

```java
    static class Node<K,V> implements Map.Entry<K,V> {
        final int hash;    //用来定位数组索引位置
        final K key;
        V value;
        Node<K,V> next;   //链表的下一个node
        Node(int hash, K key, V value, Node<K,V> next) { ... }
        public final K getKey(){ ... }
        public final V getValue() { ... }
        public final String toString() { ... }
        public final int hashCode() { ... }
        public final V setValue(V newValue) { ... }
        public final boolean equals(Object o) { ... }
    }
```
Node是HashMap的一个内部类，实现了Map.Entry接口，本质是一个映射（键值对）。

HashMap就是使用哈希表来存储的，哈希表为解决冲突，可以采用开放地址法和链地址法等来解决问题，java中HashMap采用了链地址法，简单来说就是数组加链表的结合。在每个数组元素都是一个链表结构，当数据被hash后，得到数组下标，把数据放在对应下标元素的链表上，例如：
    ``` map.put("name","earyant") ```
系统将"name"这个key的HashCode()方法得到其hashCode值，然后再通过Hash算法的后两步运算（高位运算和取模运算）来定位该键值对应的存储位置，有时候两个key会定位到相同的位置，表示发生了Hash碰撞，当然hash算法计算结果越分散均匀，Hash碰撞的概率就越小，map存取效率就会更高。
如果哈希桶数很大，即使较差的hash算法也会比较分散，如果哈希桶数组很小，就很容易发生碰撞。

# 容量
在理解Hash和扩容流程之前，我们得先了解下HashMap的几个字段。从HashMap的默认构造函数源码可知，构造函数就是对下面几个字段进行初始化，源码如下：
```
    int threshold;             // 所能容纳的key-value对极限
    final float loadFactor;    // 负载因子
    int modCount;
    int size;
```
首先，Node[] table的初始化长度length为默认16，loadFactor为负载因子，默认为0.75.threshold是HashMap所能容纳的最大数据量的Node（键值对）个数。threshold=length*loadFactor，也就是说，在数组定义好长度之后，负载因子越大，所能容纳的键值对个数越多。

* size：实际存在的键值对数量
* threshold：length*loadFactor
* modCount：记录HashMap内部结构发生裱花的次数，主要用于迭代的快速失败，内部结构变化指的是结构发生变化，比如put，但是某个key对应的value值被覆盖部署于结构变化。

在HashMap中，哈希桶数组table的长度length大小必须为2的n次方(一定是合数)，这是一种非常规的设计，常规的设计是把桶的大小设计为素数。相对来说素数导致冲突的概率要小于合数，具体证明可以参考http://blog.csdn.net/liuqiyao_01/article/details/14475159，Hashtable初始化桶大小为11，就是桶大小设计为素数的应用（Hashtable扩容后不能保证还是素数）。HashMap采用这种非常规设计，主要是为了在取模和扩容时做优化，同时为了减少冲突，HashMap定位哈希桶索引位置时，也加入了高位参与运算的过程。

这里存在一个问题，即使负载因子和Hash算法设计的再合理，也免不了会出现拉链过长的情况，一旦出现拉链过长，则会严重影响HashMap的性能。于是，在JDK1.8版本中，对数据结构做了进一步的优化，引入了红黑树。而当链表长度太长（默认超过8）时，链表就转换为红黑树，利用红黑树快速增删改查的特点提高HashMap的性能，其中会用到红黑树的插入、删除、查找等算法。本文不再对红黑树展开讨论，想了解更多红黑树数据结构的工作原理可以参考。
[http://blog.csdn.net/v_july_v/article/details/6105630。](http://blog.csdn.net/v_july_v/article/details/6105630。)

#方法
* 确定哈希桶数组索引位置。
  不管增加、删除、查找键值对，定位到哈希桶数组的位置都是很关键的第一步，
 ``` java
    // 方法一：
static final int hash(Object key) {   //jdk1.8 & jdk1.7
     int h;
     // h = key.hashCode() 为第一步 取hashCode值
     // h ^ (h >>> 16)  为第二步 高位参与运算
     return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
// 方法二：
static int indexFor(int h, int length) {  //jdk1.7的源码，jdk1.8没有这个方法，但是实现原理一样的
     return h & (length-1);  //第三步 取模运算
}
  ```

  但是，模运算的消耗还是比较大的，在HashMap中是这样做的：调用方法二来计算该对象应该保存在table数组的哪个索引处。

这个方法非常巧妙，它通过h & (table.length -1)来得到该对象的保存位，而HashMap底层数组的长度总是2的n次方，这是HashMap在速度上的优化。当length总是2的n次方时，h& (length-1)运算等价于对length取模，也就是h%length，但是&比%具有更高的效率。
* put方法
![](../img/hashMap put方法执行流程图.png)

```java
 1   public V put(K key, V value) {
 2     // 对key的hashCode()做hash
 3     return putVal(hash(key), key, value, false, true);
 4 }
 5
 6 final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
 7                boolean evict) {
 8     Node<K,V>[] tab; Node<K,V> p; int n, i;
 9     // 步骤①：tab为空则创建
10     if ((tab = table) == null || (n = tab.length) == 0)
11         n = (tab = resize()).length;
12     // 步骤②：计算index，并对null做处理
13     if ((p = tab[i = (n - 1) & hash]) == null)
14         tab[i] = newNode(hash, key, value, null);
15     else {
16         Node<K,V> e; K k;
17         // 步骤③：节点key存在，直接覆盖value
18         if (p.hash == hash &&
19             ((k = p.key) == key || (key != null && key.equals(k))))
20             e = p;
21         // 步骤④：判断该链为红黑树
22         else if (p instanceof TreeNode)
23             e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
24         // 步骤⑤：该链为链表
25         else {
26             for (int binCount = 0; ; ++binCount) {
27                 if ((e = p.next) == null) {
28                     p.next = newNode(hash, key,value,null);
                        //链表长度大于8转换为红黑树进行处理
29                     if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
30                         treeifyBin(tab, hash);
31                     break;
32                 }
                    // key已经存在直接覆盖value
33                 if (e.hash == hash &&
34                     ((k = e.key) == key || (key != null && key.equals(k))))                                            break;
36                 p = e;
37             }
38         }
40         if (e != null) { // existing mapping for key
41             V oldValue = e.value;
42             if (!onlyIfAbsent || oldValue == null)
43                 e.value = value;
44             afterNodeAccess(e);
45             return oldValue;
46         }
47     }

48     ++modCount;
49     // 步骤⑥：超过最大容量 就扩容
50     if (++size > threshold)
51         resize();
52     afterNodeInsertion(evict);
53     return null;
54 }

```

#扩容（resize）

```java
    1 void resize(int newCapacity) {   //传入新的容量
    2     Entry[] oldTable = table;    //引用扩容前的Entry数组
 3     int oldCapacity = oldTable.length;
 4     if (oldCapacity == MAXIMUM_CAPACITY) {  //扩容前的数组大小如果已经达到最大(2^30)了
 5         threshold = Integer.MAX_VALUE; //修改阈值为int的最大值(2^31-1)，这样以后就不会扩容了
 6         return;
 7     }
 8
 9     Entry[] newTable = new Entry[newCapacity];  //初始化一个新的Entry数组
10     transfer(newTable);                         //！！将数据转移到新的Entry数组里
11     table = newTable;                           //HashMap的table属性引用新的Entry数组
12     threshold = (int)(newCapacity * loadFactor);//修改阈值
13 }
```

这里就是使用一个容量更大的数组来代替已有的容量小的数组，transfer()方法将原有Entry数组的元素拷贝到新的Entry数组里。

```java
1 void transfer(Entry[] newTable) {
 2     Entry[] src = table;                   //src引用了旧的Entry数组
 3     int newCapacity = newTable.length;
 4     for (int j = 0; j < src.length; j++) { //遍历旧的Entry数组
 5         Entry<K,V> e = src[j];             //取得旧Entry数组的每个元素
 6         if (e != null) {
 7             src[j] = null;//释放旧Entry数组的对象引用（for循环后，旧的Entry数组不再引用任何对象）
 8             do {
 9                 Entry<K,V> next = e.next;
10                 int i = indexFor(e.hash, newCapacity); //！！重新计算每个元素在数组中的位置
11                 e.next = newTable[i]; //标记[1]
12                 newTable[i] = e;      //将元素放在数组上
13                 e = next;             //访问下一个Entry链上的元素
14             } while (e != null);
15         }
16     }
17 }

```


## 感谢参考
[参考](http://www.importnew.com/20386.html)
