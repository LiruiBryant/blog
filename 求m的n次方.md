---
title: 求m的n次方
date: 2017-10-24 16:45:47
tags:
---

## 初步设想
  最简单的方法，就是for循环一下。
  ```java
    double result = 1L;
     for (int i = 0; i < n; i++) {
         result *= m;
     }
  ```
  这种方法结果肯定正确，但是时间复杂度上，需要O(n)。很是不理想。于是我想着改进一下。

## 探求乘法原理
  乘法原理就是n个m相乘，例如m=10，n=5， 那么表达式为 10*10*10*10*10,有人说这不是废话嘛，当然不是，之所以展开，是因为可以改写法，如：(10*10)* (10*10)* 10,又有人说了，这不是还是废话嘛，别急往下看，你会发现(10*10)重复了。

  什么意思？

  如果单纯用for循环10的4次幂，需要 *循环4次运算*，现在我先用10*10,*第1次运算*，再用得出的结果乘以得出的结果，也就是(10*10)* (10*10),*第2次运算*。就这么简单，此次运算节省了2次，节省了百分之50啊，重大发现啊。

  如果是10的8次幂呢？单纯for循环需要8次，新的方法需要几次？*3次*。怎么算的？(10*10)* (10*10)* (10*10)* (10*10)。发现就是算10*10的4次方，算出10*10需要 *第一次*，4次方刚刚算了是2次，一共是3次，这么神奇啊，节省了5次运算啊。这是节省了百分之。。。ummmm，管他百分之多少呢，反正很多。

  照这么下去，16次方需要 *4* 次运算，32次方需要 *5* 次运算，64次方需要 *6* 次运算，重大发现，也就是说，*如果n是2的i次幂，那么就需要i次运算就可以算出m的n次方了。*

  天晴了， 世界和平了，我又拯救了世界了，可以回家睡懒觉了。

  咦，等等，如果n不是2的多少次幂怎么办？比如10的5次幂？

  ummmm，我想了想，把5拆成4+1呢？也就是10的4次幂再乘以10，这个方法可以。

  那10的6次幂就可以拆成4+2.

  7次方就可以拆成4+2+1.嗯，不错。

  于是，我写了个代码，如下

  ```java

  public class Pow {
  public static void main(String[] args) {
      int m = 5;
      int n = 134;
      long start = System.currentTimeMillis();
      System.out.println("for循环版，结果为 ： " + Double.toString(pow(m, n)));
      long end = System.currentTimeMillis();
      System.out.println("for循环版，总耗时： " + (end - start));


      start = System.currentTimeMillis();
      System.out.println("叠乘法，结果为 ： " + Double.toString(pow2(m, n)));
      end = System.currentTimeMillis();
      System.out.println("叠乘法，总耗时： " + (end - start));
  }

  /**
   * m的n次方,for循环版
   *
   * @param m 底数
   * @param n 幂数
   * @return
   */
  static double pow(int m, int n) {
      boolean negative = false;
      if (m == 0) {
          return 0;
      }
      if (n == 0) {
          return 1;
      }
      if (n < 0) {
          n = -n;
          negative = true;
      }
      /**
       * 循环乘法
       */
      double result = 1L;
      for (int i = 0; i < n; i++) {
          result *= m;
      }
      if (negative) {
          return 1 / result;
      }
      return result;
  }

  /**
   * m的n次方,折中叠乘法
   *
   * @param m 底数
   * @param n 幂数
   * @return
   */
  static double pow2(int m, int n) {
      boolean negative = false;
      if (m == 0) {
          return 0;
      }
      if (n == 0) {
          return 1;
      }
      if (n < 0) {
          n = -n;
          negative = true;
      }
//        当m是2的时候，直接位移就可以。
      if (m == 2) {
          return 2 << n;
      }
      /**
       * 叠乘法
       */
//        最后结果。
      double result = 1L;
//        定义一个指针，从右向左按位与。
      for (int i = 1; i < n; i <<= 1) {
//            按位与，如果大于0，说明此位为1,需要m的i次方。
          if ((i & n) > 0) {
              double resultTemp = 1L;
//                m的i次方不能写成循环i次的m相乘，而是m叠乘，次数为ln(i)。    例如m的8次方，就是((m*m)*(m*m))*((m*m)*(m*m)),也就是 ((m*m)2)2
              for (int j = 1; j <= i; j <<= 1) {
                  if (j == 1) {
                      resultTemp = m;
                  } else {
                      if (greatThanMax(resultTemp, resultTemp)) {
                          return Double.MAX_VALUE;
                      }
                          resultTemp *= resultTemp;
                  }
              }
              if (greatThanMax(resultTemp, result)) {
                  return Double.MAX_VALUE;
              }
              result *= resultTemp;
          }
      }

      if (negative) {
          return 1 / result;
      }
      return result;
  }


  /**
   * 检查两个数相乘是否大于double的最大值。
   *
   * @param m
   * @param n
   * @return
   */
  public static boolean greatThanMax(double m, double n) {
      return (m > (Double.MAX_VALUE / n));
  }
}
```

测试结果为

  > for循环版，结果为 ： 4.591774807899563E93
    for循环版，总耗时： 1
    叠乘法，结果为 ： 4.591774807899562E93
    叠乘法，总耗时： 0

## 彩蛋
  如果m是2的话，只需要返回2<<n就行了。自行判断是否越界。
