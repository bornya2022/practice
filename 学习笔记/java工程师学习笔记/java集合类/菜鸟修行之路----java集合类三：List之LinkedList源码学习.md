# 菜鸟修行之路----java集合类三：List之LinkedList源码学习

​        

​        **LinkedList** 是用链表结构存储数据的，很适合数据的动态插入和删除，随机访问和遍历速度比较

慢。另外，他还提供了 List 接口中没有定义的方法，专门用于操作表头和表尾元素，可以当作堆栈、队列和双向队列使用

## 1.LinkedList概述

   LinkList主要具有以下特点：

- 排列有序，可重复
- 查询速度慢，增删快，add()和remove()方法快
- 线程不安全
- 底层数据结构采用双向循环链表实现

对于解决其线程不安全问题，可以采用线程同步的方式：创建List时构造一个同步的List

```java
List list = Collections.synchronizedList(new LinkedList(...));
```

## 2.继承关系图

![1583286912555](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1583286912555.png)

通过继承关系图，可以看出：

- 实现了**List接口**，提供了基础的添加、删除、遍历等操作
- 还实现了**Queue**和**Deque**接口，作为双端队列使用
- 实现了**Serializable接口**，可以被序列化。
- 实现了**Cloneable接口**，可以被克隆。

## 3.源码分析

### 3.1 成员变量

```java
public class LinkedList<E>
    extends AbstractSequentialList<E>
    implements List<E>, Deque<E>, Cloneable, java.io.Serializable
{
    //元素个数
    transient int size = 0;

    /**
     *链表首节点
     * Pointer to first node.
     * Invariant: (first == null && last == null) ||
     *            (first.prev == null && first.item != null)
     */
    transient Node<E> first;

    /**
     *链表尾结点
     * Pointer to last node.
     * Invariant: (first == null && last == null) ||
     *            (last.next == null && last.item != null)
     */
    transient Node<E> last;
    。。。。。
}

```

通过上诉代码可以得知：LinkedList的主要属性定义了元素个数以及链表的首尾结点。

### 3.2 构造方法

```java
/**
     * Constructs an empty list.
     */
    public LinkedList() {
    }

    /**
     * Constructs a list containing the elements of the specified
     * collection, in the order they are returned by the collection's
     * iterator.
     *
     * @param  c the collection whose elements are to be placed into this list
     * @throws NullPointerException if the specified collection is null
     */
    public LinkedList(Collection<? extends E> c) {
        this();
        addAll(c);//参见后文核心方法分析
    }
```

通过上诉源码可以看出：LinkeList有2个构造方法，一个空的构造方法和一个带集合参数的构造方法。

**与ArrayList的源码对比，可以发现LinkedList少了一个设置容量的初始化类，原因很简单。LinkedList采用的时链表实现，添加新元素时通过链接新节点实现的，它的容量可以说是随着元素个数实现动态变化的。**

### 3.3 核心内部类

```java
//双向循环链表
private static class Node<E> {
        E item;
        Node<E> next;
        Node<E> prev;

        Node(Node<E> prev, E element, Node<E> next) {
            //元素
            this.item = element;
            this.next = next;
            this.prev = prev;
        }
    }
```

该类就是LinkedList的底层数据结构，这部分可以说时LinkedList的核心，是LinkedList中用来真正存储数据的数据结构（和ArrayLisy中elementData数组一致）。

具体的链表的知识参见数据结构。

### 3.4 核心方法

LinkedList实现了List接口，所有它和ArrayList一致有List的基础操作的方法，不在具体分析。接下来就重点分析以下它的增删改查的相关方法。

#### 3.4.1添加元素方法

对于在链表中添加元素，只有以下3种情况：

- 在链表首部添加元素
- 在链表尾部添加元素
- 在列表中间添加元素

**在链表首部添加元素**

```java
 /**
     * Inserts the specified element at the beginning of this list.
     *
     * @param e the element to add
     */
    public void addFirst(E e) {
        linkFirst(e);
    }

    /**
     * Links e as first element.
     */
    private void linkFirst(E e) {
        //首节点
        final Node<E> f = first;
        //创建新节点，并且满足newNode的next是首节点
        final Node<E> newNode = new Node<>(null, e, f);
        //让新节点作为新的首节点
        first = newNode;
        // 判断是不是第一个添加的元素
        // 如果是就把last也置为新节点
        // 否则把原首节点的prev指针置为新节点
        if (f == null)
            last = newNode;
        else
            f.prev = newNode;
        //元素个数加1
        size++;
        //修改次数加一，支持fail-fast（快速失败，错误检查机制）
        modCount++;
    }
```

**在链表尾部添加元素**

```java
/**
     * Appends the specified element to the end of this list.
     *
     * <p>This method is equivalent to {@link #add}.
     *
     * @param e the element to add
     */
    public void addLast(E e) {
        linkLast(e);
    }
    
    /**
     * Links e as last element.
     */
    // 从队列尾添加元素
    void linkLast(E e) {
          // 队列尾节点
          final Node<E> l = last;
          // 创建新节点，新节点的prev是尾节点
          final Node<E> newNode = new Node<>(l, e, null);
           // 让新节点成为新的尾节点
           last = newNode;
           // 判断是不是第一个添加的元素
           // 如果是就把first也置为新节点
           // 否则把原尾节点的next指针置为新节点
           if (l == null)
               first = newNode;
           else
               l.next = newNode;
           // 元素个数加1
           size++;
           // 修改次数加1
           modCount++;
    }
```

**在列表中间添加元素**：在指定位置添加元素

```java
/**
     * Inserts the specified element at the specified position in this list.
     * Shifts the element currently at that position (if any) and any
     * subsequent elements to the right (adds one to their indices).
     *
     * @param index index at which the specified element is to be inserted
     * @param element element to be inserted
     * @throws IndexOutOfBoundsException {@inheritDoc}
     */
    // 在指定index位置处添加元素
    public void add(int index, E element) {
        // 判断是否越界
        checkPositionIndex(index);
        // 判断结点位置，如果index是在队列尾节点之后的一个位置
       // 把新节点直接添加到尾节点之后
       // 否则调用linkBefore()方法在中间添加节点
        if (index == size)
            //添加到链表尾部
            linkLast(element);
        else
            linkBefore(element, node(index));
    }
    //越界检查
    private void checkPositionIndex(int index) {
        if (!isPositionIndex(index))
            throw new IndexOutOfBoundsException(outOfBoundsMsg(index));
    }
    /**
     * Tells if the argument is the index of a valid position for an
     * iterator or an add operation.
     */
    private boolean isPositionIndex(int index) {
        return index >= 0 && index <= size;
    }
    
    /**
     * Inserts element e before non-null Node succ.
     *在节点succ之前添加元素
     */
    void linkBefore(E e, Node<E> succ) {
        // assert succ != null;
         // succ是待添加节点的后继节点， 找到待添加节点的前置节点
        final Node<E> pred = succ.prev;
        // 在其前置节点和后继节点之间创建一个新节点（添加的新元素）
        final Node<E> newNode = new Node<>(pred, e, succ);
        // 修改后继节点的前置指针指向新节点
        succ.prev = newNode;
        // 判断前置节点是否为空
        // 如果为空，succ原来为链接表的首节点，添加的元素成为新的首节点，修改first指针
        // 否则修改前置节点的next为新节点
        if (pred == null)
            first = newNode;
        else
            pred.next = newNode;
         // 修改元素个数
        size++;
        // 修改次数加1
        modCount++;
    }
   
   /**
     * Returns the (non-null) Node at the specified element index.
     */
    // 寻找index位置的节点，也就是succ的结点位置
    Node<E> node(int index) {
        // assert isElementIndex(index);
        // 因为是双链表
      // 所以根据index是在前半段还是后半段决定从前遍历还是从后遍历
       // 这样index在后半段的时候可以少遍历一半的元素，提高效率
        if (index < (size >> 1)) {
            //在前半段
            Node<E> x = first;
            for (int i = 0; i < index; i++)
                x = x.next;
            return x;
        } else {
            //在后半段
            Node<E> x = last;
            for (int i = size - 1; i > index; i--)
                x = x.prev;
            return x;
        }
    }  
```

通过对上诉添加方法的分析可以得出：

- 在链表首尾添加元素很高效，时间复杂度为O(1)。
- 在中间添加元素比较低效，首先要先找到插入位置的节点，再修改前后节点的指针，时间复杂度为O(n)。

#### 3.4.2 删除方法

删除和新增类似，在哪一部分增加，就可以在那一部分删除。还是只有以下3种情况：

- 在链表首部删除元素
- 在链表尾部删除元素
- 在列表中间删除元素

**在链表首部删除元素**

```java
/**
     * Removes and returns the first element from this list.
     *
     * @return the first element from this list
     * @throws NoSuchElementException if this list is empty
     *在链表首部删除元素，如果链表为空，没有首部元素，抛出异常
     */
    public E removeFirst() {
        final Node<E> f = first;
        if (f == null)
            throw new NoSuchElementException();
        return unlinkFirst(f);
    }
    /**
     * Unlinks non-null first node f.
     */
    // 删除首节点，并且返回删除的值
    private E unlinkFirst(Node<E> f) {
        // assert f == first && f != null;
        // 获取到首节点的元素值，然后在重置首节点，将首节点的next作为新的首节点，如果该链表种本来就         //只有一个元素，则删除后为空链表，首尾节点为空。
        final E element = f.item;
        final Node<E> next = f.next;
        f.item = null;
        f.next = null; // help GC
        first = next;
        if (next == null)
            last = null;
        else
            next.prev = null;
        size--;
        modCount++;
        return element;
    }
```

**在链表尾部删除元素**

在尾部删除和在首部删除思路一致，不再缀诉。

```java
  /**
     * Removes and returns the last element from this list.
     *
     * @return the last element from this list
     * @throws NoSuchElementException if this list is empty
     */
   //**在链表尾部删除元素**
    public E removeLast() {
        final Node<E> l = last;
        if (l == null)
            throw new NoSuchElementException();
        return unlinkLast(l);
    }
    /**
     * Unlinks non-null last node l.
     */
    private E unlinkLast(Node<E> l) {
        // assert l == last && l != null;
        final E element = l.item;
        final Node<E> prev = l.prev;
        l.item = null;
        l.prev = null; // help GC
        last = prev;
        if (prev == null)
            first = null;
        else
            prev.next = null;
        size--;
        modCount++;
        return element;
    }
```

**在列表中间删除元素**：删除指定位置的元素

```java
/**
     * Removes the element at the specified position in this list.  Shifts any
     * subsequent elements to the left (subtracts one from their indices).
     * Returns the element that was removed from the list.
     *
     * @param index the index of the element to be removed
     * @return the element previously at the specified position
     * @throws IndexOutOfBoundsException {@inheritDoc}
     */
    public E remove(int index) {
        //越界检查
        checkElementIndex(index);
        //查找index位置的节点，并将其作为参数传给unlink方法
        return unlink(node(index));
    }

    /**
     * Unlinks non-null node x.
     */
    //删除指定位置x的节点，并且将x的值返回，删除后链表的节点的指针链接和新增类似。
    E unlink(Node<E> x) {
        // assert x != null;
        final E element = x.item;
        final Node<E> next = x.next;
        final Node<E> prev = x.prev;
         // 如果前置节点为空
        // 说明是首节点，让first指向x的后置节点
        // 否则修改前置节点的next为x的后置节点
        if (prev == null) {
            first = next;
        } else {
            prev.next = next;
            x.prev = null;
        }
        // 如果后置节点为空
        // 说明是尾节点，让last指向x的前置节点
        // 否则修改后置节点的prev为x的前置节点
        if (next == null) {
            last = prev;
        } else {
            next.prev = prev;
            x.next = null;
        }

        x.item = null;
        size--;
        modCount++;
        return element;
    }
```

通过上述方法的分析可以得知：

- 在链表首尾删除元素很高效，时间复杂度为O(1)。
- 在中间删除元素比较低效，首先要找到删除位置的节点，再修改前后指针，时间复杂度为O(n)。

#### 3.4.3 拓展结构

LinkedList是以双向链表为基础的，所有LinkedList也可以实现栈：在只在链表的首尾部操作元素

**在首部删除，在尾部新增。（一进一出）

```java
public void push(E e) {
    addFirst(e);
}
 
public E pop() {
    return removeFirst();
}
```

### 4.LinkedList的应用

**应用实例一:增删改查**

```java
public class LinkListTest {
    public static void main(String[] args){
        LinkedList linkedList=new LinkedList();
        //添加元素
        linkedList.add("sad");
        linkedList.add("dfasd");
        //在首部添加元素
        linkedList.addFirst("asda");
        //在尾部添加元素
        linkedList.addLast("dfasd");
        //在指定位置添加元素
        linkedList.add(2,"dasd");
        //删除首部元素
        linkedList.removeFirst();
        //删除指定位置元素
        linkedList.remove(2);
        //删除尾部元素
        linkedList.removeLast();
        //元素个数
        int a=linkedList.size();
        //获取索引为3的元素并且将其转化为字符串
        String item=linkedList.get(3).toString();
    }
}
```







**后记：修行之路艰辛，与君共勉。**

​                                                                                                                                                             ---2020年春：成都



