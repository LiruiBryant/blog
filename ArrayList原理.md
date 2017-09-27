---
title: ArrayList原理
date: 2017-09-23 14:36:39
tags: java
---

## 实现方式
  存储数据是通过数组： transient Objectp[] elementData
  size字段： 存储整个数组的大小，
    > 注意的是： size是每次add和remove都会自增和自减的，所以增加null也会size+1.

  * 是否允许空：
    允许
  * 是否允许重复数据：
    允许
  * 是否有序：
    是
  * 是否线程安全：
    不是

## 默认大小 : 10
  ```java
  public ArrayList(){
    this(10);
  }
  ```

## 扩容
```java

  public void ensureCapacity( int minCapacity){
    modCount++;
    int oldCapacity = elementData.length;
    if (minCapacity > oldCapacity) {
        Object oldData[] = elementData;
        int newCapacity = (oldCapacity * 3)/2 + 1;
            if (newCapacity < minCapacity)
        newCapacity = minCapacity;
               // minCapacity is usually close to size, so this is a win:
               elementData = Arrays.copyOf(elementData, newCapacity);
             }
  }

```
## 删除元素
  * 根据下标删除。
  * 根据元素删除。
    > 删除跟元素匹配的 *第一个* 元素

  ```java
      int numMoved = size - index - 1;

      if (numMoved > 0)

      System.arraycopy(elementData, index+1, elementData, index,

               numMoved);

      elementData[--size] = null; // Let gc do its work
   ```
   * 1、把指定元素后面位置的所有元素，利用System.arraycopy方法整体向前移动一个位置
   * 2、最后一个位置的元素指定为null，这样让gc可以去回收它
   > 因为删除的时候，需要将后面所有元素向前移动一个位置，所以删除的时候，很消耗性能。


## 插入元素
  * add(T t)
  * add(int position,T t)
    > 插入时候会将所有后面的元素向后移动一个位置，很消耗性能。
    
## ArrayList的优缺点
 * 底层以数组方式存储，是一种随机访问方式，并且实现了RandomAccess接口，因此查找十分快速。
 * 添加很方便，往数组的最后添加一个就行了。
 
 * 删除和添加的时候涉及到移动后面所有数据，性能消耗比较大。    
 
## 线程安全
   > 不是线程安全的，所有方法都没有Synchronized修饰，在并发情况下会出现安全问题。可以使用Collections.synchronizedList方式
   
##  为什么ArrayList的elementData是用transient修饰的？
   > private transient Object[] elementData;
   
   >ArrayList 实现了Serializable接口，所以是可以序列化的。但是elementData不一定是满的，没必要全部序列化。所以ArrayList重写了writeObject方法实现，增快了序列化的速度，见笑了序列化后的大小：
   ```java
    private void writeObject(java.io.ObjectOutputStream s)
            throws java.io.IOException{
    // Write out element count, and any hidden stuff
    int expectedModCount = modCount;
    s.defaultWriteObject();
            // Write out array length
           s.writeInt(elementData.length);
        // Write out all elements in the proper order.
    for (int i=0; i<size; i++)
               s.writeObject(elementData[i]);
        if (modCount != expectedModCount) {
               throw new ConcurrentModificationException();
        }
    }
   ```
    