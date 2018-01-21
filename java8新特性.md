---
title: java8新特性
date: 2018-01-20 09:47:36
tags: java8
---

# java8新特性

## lambda表达式

> java8接受一个函数作为另一个函数的参数传入进去

Arrays.asList( "a", "b", "d" ).forEach( e -> System.out.println( e ) ); 参数的类型是编译器推断出来的

> Lambda可以引用类的成员变量与局部变量（如果这些变量不是final的话，它们会被隐含的转为final，这样效率更高）。

## 默认方法和静态方法

> 在JVM中，默认方法的实现是非常高效的，并且通过字节码指令为方法调用提供了支持。默认方法允许继续使用现有的Java接口，而同时能够保障正常的编译过程。这方面好的例子是大量的方法被添加到java.util.Collection接口中去：stream()，parallelStream()，forEach()，removeIf()，......

## 重复注解

## Java编译器的新特性

```java
public class ParameterNames {
    public static void main(String[] args) throws Exception {
        Method method = ParameterNames.class.getMethod( "main", String[].class );
        for( final Parameter parameter: method.getParameters() ) {
            System.out.println( "Parameter: " + parameter.getName() );
        }
    }
}
```

> Parameter: args

对于有经验的Maven用户，通过maven-compiler-plugin的配置可以将-parameters参数添加到编译器中去。

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.1</version>
    <configuration>
        <compilerArgument>-parameters</compilerArgument>
        <source>1.8</source>
        <target>1.8</target>
    </configuration>
</plugin>
```

## Java 类库的新特性

### Optional

平时用的判空：

```java
public void get(String name){
  if (name == null){
      return "";
  }else{
      return name;
  }
}
```

改成Optional：

```java
public void get(String name){
  Optional<String > names = Opntional.of(name);
  if (names.isPresent()){
      return names.get();
  }else{
      return "";
  }
}
```

如果单纯改成这样，还不如直接写成 ==null，看起来更加繁琐。 应该改成这样：

```java
public void get(String name){
  return Opntional.of(name).get().orElse("");

}
```

### Stream

```java
final long totalPointsOfOpenTasks = tasks
    .stream()
    .filter( task -> task.getStatus() == Status.OPEN )
    .mapToInt( Task::getPoints )
    .sum();
```

#### filter

> 对于Stream中包含的元素使用给定的过滤函数进行过滤操作，新生成的Stream只包含符合条件的元素；

#### map

> 映射转换

#### mapTo*

> 映射转换成 _的类型，_ 包括Integer、Double、Long

#### flatMap

> 合并集合,和map类似，不同的是其每个元素转换得到的是Stream对象，会把子Stream中的元素压缩到父集合中；

```
{{1,2}，{3,4}，{5,6}} - > flatMap - > {1,2,3,4,5,6}

{'a'，'b'}，{'c'，'d'}，{'e'，'f'}} - > flatMap - > {'a'，'b'，'c' D"， 'E'， 'F'}
```

#### flatMapTo*

#### distinct

> 对于Stream中包含的元素进行去重操作（去重逻辑依赖元素的equals方法），新生成的Stream中没有重复的元素；

> #### sorted

#### peek

> 监控器

提供一个Consumer消费函数，返回一个新的Stream，如果只写 Stream.of("1","2","3").peek(e-> System.out.Println(e)) 是不会打印结果的 新Stream每个元素被**消费**的时候都会执行给定的消费函数； Stream.of("1","2","3").peek(e-> System.out.Println(e)).collect(Collectors.toList());

#### limit

对一个Stream进行截断操作，获取其前N个元素，如果原Stream中包含的元素个数小于N，那就获取其所有的元素；

#### skip

返回一个丢弃原Stream的前N个元素后剩下元素组成的新Stream，如果原Stream中包含的元素个数小于N，那么返回空Stream；

#### forEach

> 遍历

#### forEachOrdered

> 与forEach一样，只不过每次遍历结果都是相同的，forEach不是固定的序列

#### generate

给定一个生成函数，生成一个无限的队列 Stream.generate(Math::random);

#### iterate

也是生成无限长度的Stream，和generator不同的是，其元素的生成是重复对给定的种子值(seed)调用用户指定函数来生成的。其中包含的元素可以认为是：seed，f(seed),f(f(seed))无限循环

Stream.iterate(1, item -> item + 1).limit(10).forEach(System.out::println);

#### collect

> 汇聚函数

方法参数              | 返回类型                   | 作用                                                      | 示例
----------------- | ---------------------- | ------------------------------------------------------- | ------------------------------------------------------------------------------------------------
toList            | List`<T >`             | 把流中的参数汇聚成List集合                                         | 示例:List`<Menu>` menus=Menu.getMenus.stream().collect(Collectors.toList())
toSet             | Set`<T>`               | 把流中所有元素收集到Set中,删除重复项                                    | Set`<Menu>` menus=Menu.getMenus.stream().collect(Collectors.toSet())
toCollection      | Collection`<T>`        | 把流中所有元素收集到给定的供应源创建的集合中                                  | `ArrayList<Menu> menus=Menu.getMenus.stream().collect(Collectors.toCollection(ArrayList::new))`
Counting          | Long                   | 计算流中元素个数                                                | Long count=Menu.getMenus.stream().collect(Collectors.counting());
SummingInt        | Integer                | 对流中元素的一个整数属性求和                                          | Integer count=Menu.getMenus.stream().collect(Collectors.summingInt(Menu::getCalories))
averagingInt      | Double                 | 计算流中元素integer属性的平均值                                     | Double averaging=Menu.getMenus.stream().collect((Collectors.averagingInt(Menu::getCalories))
Joining           | String                 | 连接流中每个元素的toString方法生成的字符串                               | String name=Menu.getMenus.stream().map(Menu::getName).collect(Collectors.joining(", "))
maxBy             | Optional`<T>`          | 一个包裹了流中按照给定比较器选出的最大元素的optional 如果为空返回的是Optional.empty() | Optional`<Menu>` fattest=Menu.getMenus.stream().collectCollectors.maxBy(Menu::getCalories))
minBy             | Optional`<T>`          | 一个包裹了流中按照给定比较器选出的最大元素的optional如果为空返回的是Optional.empty()  | Optional`<Menu>` lessest=Menu.getMenus.stream().collect(minBy(Menu::getCalories))
Reducing          | 归约操作产生的类型              | 从一个作为累加器的初始值开始,利用binaryOperator与流中的元素逐个结合,从而将流归约为单个值    | int count=Menu.getMenus.stream().collect(reducing(0,Menu::getCalories,Integer::sum));
collectingAndThen | 转换函数返回的类型              | 包裹另一个转换器,对其结果应用转换函数                                     | Int count=Menu.getMenus.stream().collect(collectingAndThen(toList(),List::size))
groupingBy        | Map`<K,List<T>>`       | 根据流中元素的某个值对流中的元素进行分组,并将属性值做为结果map的键                     | Map`<Type,List<Menu>>` menuType=Menu.getMenus.stream().collect(groupingby(Menu::getType))
partitioningBy    | Map`<Boolean,List<T>>` | 根据流中每个元素应用谓语的结果来对项目进行分区                                 | Map`<Boolean,List<Menu>>` menuType=Menu.getMenus.stream().collect(partitioningBy(Menu::isType));

#### reduce

- `Optional<t> reduce(BinaryOperator<t> accumulator);`

List `<integer>` ints = Lists.newArrayList(1,2,3,4,5,6,7,8,9,10); System.out.println("ints sum is:" + ints.stream().reduce((sum, item) -> sum + item).get());

可以看到reduce方法接受一个函数，这个函数有两个参数，第一个参数是上次函数执行的返回值（也称为中间结果），第二个参数是stream中的元素，这个函数把这两个值相加，得到的和会被赋值给下次执行这个函数的第一个参数。要注意的是：**第一次执行的时候第一个参数的值是Stream的第一个元素，第二个参数是Stream的第二个元素**。这个方法返回值类型是Optional，这是Java8防止出现NPE的一种可行方法，后面的文章会详细介绍，这里就简单的认为是一个容器，其中可能会包含0个或者1个对象。

- T reduce(T identity, BinaryOperator`<t>` accumulator);
  这个定义上上面已经介绍过的基本一致，不同的是：它允许用户提供一个循环计算的初始值，如果Stream为空，就直接返回该值。而且这个方法不会返回Optional，因为其不会出现null值。

#### count

> 个数

#### allMatch

是不是所有都满足给定的条件

#### anyMatch

是不是存在某一个元素满足给定的条件

#### findFirst

返回第一个，返回的是Optional值

#### noneMatch

是否没有一个元素满足给定的条件

#### max和min

给定的比较条件，返回最大最小值

[参考](http://ifeve.com/stream/)
