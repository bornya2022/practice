# 菜鸟修行之路----java集合类一：集合概述

​        java集合类是一个非常重要的一个模块，接下来的几篇博客将对于集合这一块的内容继续整理和记忆。

## 1.总体架构图

​      java集合类存放于Java.util包中，总体架构图如下：

![](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1583113966447.png)

​        通过总体架构图可以得知，java集合框架提供了3个顶层接口：Collection，Map，Iterator，其中Collection和Map是java所有集合类的根接口。

- Collection：Collection 是高度抽象出来的集合，它包含了集合的基本操作和属性。Collection包含了List和Set以及Queue三个子接口。

  ​          **List**：有序队列，每一个元素都有它的索引。第一个元素的索引值是0。

  ​          **Set**：不能有重复元素的集合。

  ​         **Queue**：队列，两端出入的LIst。

- Iterator：迭代器，遍历集合的工具，可以通过迭代器遍历集合中的数据。

- Map：是映射表的基础接口，基于**key-value键值对**。

框架图左下角的工具类：Arrays和Collections，这2个类是操作数组和集合的工具类。

## 2.java集合实现类概述

### 2.1Collection

Collection接口是处理对象集合的根接口，包含了集合的基本操作和属性。

常用方法：

| 方法        | 说明                               |
| ----------- | ---------------------------------- |
| add()       | 添加一个元素到集合中               |
| addAll()    | 将指定集合中的所有元素添加到集合中 |
| contains()  | 检测集合中是否存在指定元素         |
| toArray（） | 返回一个表示集合的数组             |
| size（）    | 获取集合的大小，元素个数           |
| remove（）  | 删除集合中的指定元素               |

其他方法参考Java的API文档。

Collection包含了List和Set以及Queue三个子接口。

####  2.1.1List接口

​        List集合代表一个有序集合，集合中每个元素都有其对应的顺序索引。List集合允许使用重复元素，可以通过索引来访问指定位置的集合元素。

**List的实现类**：

| 实现类     | 底层结构     | 说明                                                         |
| ---------- | ------------ | ------------------------------------------------------------ |
| ArrayList  | 数组         | 排列有序，可重复<br>查询速度快，增删慢，getter()和setter()方法快<br>线程不安全<br>扩充容量=当前容量*1.5+1 |
| LinkedList | 双向循环链表 | 排列有序，可重复<br>查询速度慢，增删慢，add()和remove()方法快<br>线程不安全 |
| Vector     | 数组         | 排列有序，可重复<br>查询速度快，增删慢<br>线程安全，效率低<br>扩容时，默认：扩充容量为当前容量的一倍 |

简单实例：

```java
public class TestList {

     private List<String> list = new ArrayList<String>();
     public TestList(){
         //写入数据，调用的是ArrayList实现类.
         list.add(1, "测试数据1");
         list.add(2, "测试数据2");
         list.add(3, "测试数据3");
         list.add(4, "测试数据4");
         //输出全部元素
         System.out.println(list);
    }
    public static void main(String [] args){
        TestList()
    }
}
```



#### 2.1.2 Set接口

Set是一种不包含重复的元素的Collection，无序，即任意的两个元素e1和e2都有e1.equals(e2)=false，Set最多有一个null元素。

**Set的实现类**：

| 实现类      | 底层结构    | 说明                                              |
| ----------- | ----------- | ------------------------------------------------- |
| HashSet     | Hash表      | 排列无序，不可重复<br>存取速度快<br>内部是HashMap |
| TreeSet     | 二叉树      | 排列无序,不可重复<br>内部是TreeMap的SortedSet     |
| LinkHashSet | LinkhashMap | 采用hash表存储，并用双向链表记录插入顺序          |

### 2.2 Map

​          Map与List、Set接口不同，它是由一系列键值对组成的集合，提供了key到Value的映射。

**它也没有继承Collection**。

​      在Map中它保证了key与value之间的一一对应关系。也就是说一个key对应一个value，所以它**不能存在相同的key值，当然value值可以相同**。

​     **Map的实现类**：

| 实现类    | 底层结构 | 说明                                                         |
| --------- | -------- | ------------------------------------------------------------ |
| HashMap   | 哈希表   | 键不可重复，值可以重复<br>线程不安全<br>允许key和value的值为null |
| hashtable | 哈希表   | 键不可重复，值可以重复<br/>线程安全<br/>key和value的值都不能为null |
| TreeMap   | 二叉树   | 键不可重复，值可以重复                                       |

简单实例：

```java
public class TestMap {

     private Map<Integer,String> mp = new HashMap<Integer, String>();
     public TestMap(){
         //写入数据，调用的是HashMap实现类，所有键值不可重复
         mp.put(1, "测试数据1");
         mp.put(2, "测试数据2");
         mp.put(3, "测试数据3");
         mp.put(4, "测试数据4");
         //输出全部元素
         System.out.println(mp);
        //输出指定键值为1的元素
        System.out.println(mp.get(1));
     }
    public static void main(String [] args){
        TestMap()
    }
}
```





**后记：修行之路艰辛，与君共勉。**

​                                                                                                                                                             ---2020年春：成都