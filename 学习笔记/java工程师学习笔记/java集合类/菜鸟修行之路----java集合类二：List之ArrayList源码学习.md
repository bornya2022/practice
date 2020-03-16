# 菜鸟修行之路----java集合类二：List之ArrayList源码学习

**前言**：修行之路艰辛，对于源码的学习更是难上加难。但是也不能因此的放弃。

修行，本就是一次奋斗不息的旅行。

源码的学习，这个一块很难，特别是像我们这种英文比较差一点的，直接看源码和英文注释，感觉会看到怀疑人生。源码本身写的比较晦涩难懂，但是无论某个框架还是类的源码他的架构是不会变。只要理清其中的关系，掌握架构，源码学习就成功了一半。

只要有java语言基础的人都知道，java可以说类为基本单位。无论多么庞大复杂的框架，都是一个一个的类构成。

然而每个类的基本架构都是相同的，大体包含以下几个方面：

- 继承的类以及实现的接口
- 成员变量
- 构造方法
- 核心方法
- 内部类       

后面对于集合类中重要实现类的源码分析，就会从以上几个方面进行。





 **List集合代表一个有序集合，集合中每个元素都有其对应的顺序索引。List集合允许使用重复元素，可以通过索引来访问指定位置的集合元素。**

​       List接口为Collection直接接口。

​      List所代表的是有序的Collection，即它用某种特定的插入顺序来维护元素顺序。用户可以对列表中每个元素的插入位置进行精确地控制，同时可以根据元素的整数索引（在列表中的位置）访问元素，并搜索列表中的元素。

​      实现List接口的集合主要有：**ArrayList、LinkedList、Vector、Stack**

## 1.ArrayList

ArrayList是一种以数组实现的List，与数组相比，它具有动态扩展的能力，因此也可称之为动态数组。

它具有以下特点：

- 排列有序，可重复
- 查询速度快，增删慢，getter()和setter()方法快
- 线程不安全
- 扩充容量=当前容量*1.5+1

### 1.1 继承关系图

![1583196509895](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1583196509895.png)

由继承关系图可以得知：

- 实现了List接口，提供了基础的添加、删除、遍历等操作。
- 实现了RandomAccess接口，提供了随机访问的能力。
- 实现了Cloneable接口，可以被克隆。
- 实现了Serializable接口，可以被序列化。

### 1.2 源码分析

#### 1.2.1**成员变量**

```java
//ArrayList继承AbstractList类，实现RandomAccess, Cloneable, java.io.Serializable接口
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable
{
    private static final long serialVersionUID = 8683452581122892189L;

    /**
     * Default initial capacity.
     *默认容量为10 ，即我们通过new ArrayList创建的集合的容量为10
     */
    private static final int DEFAULT_CAPACITY = 10;

    /**
     * Shared empty array instance used for empty instances.
     *传入数组为空（容量为0）的时候使用。
     *空数组，通过new ArrayList(0)的得到就是这个空数组。
     */
    private static final Object[] EMPTY_ELEMENTDATA = {};

    /**
     * Shared empty array instance used for default sized empty instances. We
     * distinguish this from EMPTY_ELEMENTDATA to know how much to inflate when
     * first element is added.
     *也是空数组，我们通过new ArrayList()首先得到就是这个空数组，不过在添加了第一个元素后，这个数组就是      *初始化为容量=10（默认容量）的数组。
     */
    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};

    /**
     * The array buffer into which the elements of the ArrayList are stored.
     * The capacity of the ArrayList is the length of this array buffer. Any
     * empty ArrayList with elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA
     * will be expanded to DEFAULT_CAPACITY when the first element is added.
     *存储ArrayList元素的数组，数组的大小就是ArrayList的实际容量
     *采用transient目的使elementData不被序列化。
     */
    transient Object[] elementData; // non-private to simplify nested class access

    /**
     * The size of the ArrayList (the number of elements it contains).
     *
     * @serial
     *集合中的元素个数（可以理解为实际大小）
     */
    private int size;
    ....
    .....
    .....
}
```

#### **1.2.2构造方法**

Arraylist的容量设置，由以下2个方法确定：

带容量参数initialCapacity的构造方法以及无参构造方法

```java
/**
     * Constructs an empty list with the specified initial capacity.
     *
     * @param  initialCapacity  the initial capacity of the list
     * @throws IllegalArgumentException if the specified initial capacity
     *         is negative
     *
     */
    public ArrayList(int initialCapacity) {
        if (initialCapacity > 0) {
            this.elementData = new Object[initialCapacity];
        } else if (initialCapacity == 0) {
            this.elementData = EMPTY_ELEMENTDATA;
        } else {
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        }
    }
/**
 * Constructs an empty list with an initial capacity of ten.
 */
public ArrayList() {
    this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
}
```

通过对上诉方法可以得知：

-  当通过new ArrayList（）创建集合时，调用无参的构造方法。创建一个空数组，然后在添加了第一个元素   后，数组容量设定为：10
- 当通过new ArrayList(int 参数1)创建集合时，根据参数1的取值，选择不同的容量，若参数1为0，则创建空数组，若参数1不等于0，则参数1就是该集合的容量。

**ArrayList(Collection<? extends E> c)**        

​        传入集合并初始化elementData，使用拷贝把传入集合的元素拷贝到elementData数组中，如果元素个数为0，则转化为EMPTY_ELEMENTDATA空数组。

```java

/**
 * Constructs a list containing the elements of the specified
 * collection, in the order they are returned by the collection's
 * iterator.
 *
 * @param c the collection whose elements are to be placed into this list
 * @throws NullPointerException if the specified collection is null
 */
public ArrayList(Collection<? extends E> c) {
    //集合转化为数组
    elementData = c.toArray();
    if ((size = elementData.length) != 0) {
        // c.toArray might (incorrectly) not return Object[] (see 6260652)
        // 判断c.toArray()返回的是不是Object[]类型，如果不是，重新拷贝成Object[].class类型
        if (elementData.getClass() != Object[].class)
            elementData = Arrays.copyOf(elementData, size, Object[].class);
    } else {
        // replace with empty array.
        this.elementData = EMPTY_ELEMENTDATA;
    }
}
```
总结：通过构造方法的分析可以得知：

- 集合容量可以通过创建集合时指定，如果没有指定，则在添加完第一个元素后默认指定集合容量为10.
- 集合中的数据最终存放在elementData这个数组中。

#### 1.2.3核心方法

**常用方法列表**：

| 方法                           | 功能                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| add(Object element)            | 向列表的尾部添加指定的元素。                                 |
| size()                         | 返回列表中的元素个数。                                       |
| get(int index)                 | 返回列表中指定位置的元素，index从0开始。                     |
| add(int index, Object element) | 在列表的指定位置插入指定元素。                               |
| set(int i, Object element)     | 将索引i位置元素替换为元素element并返回被替换的元素。         |
| clear()                        | 从列表中移除所有元素。                                       |
| isEmpty()                      | 判断列表是否包含元素，不包含元素则返回 true，否则返回false。 |
| contains(Object o)             | 如果列表包含指定的元素，则返回 true。                        |
| remove(int index)              | 移除列表中指定位置的元素，并返回被删元素。                   |
| remove(Object o)               | 移除集合中第一次出现的指定元素，移除成功返回true，否则返回false |
| iterator()                     | 返回按适当顺序在列表的元素上进行迭代的迭代器                 |



对于源码，重点学习ArrayList的3类核心方法：add,get,remove.

**add(E e)方法**：

添加元素到末尾，平均时间复杂度为O(1)。

对于该方法的重点就是扩容检查，因为通过上文可以得知，arraylist是固定容量的，所以在添加元素在末尾是需要进行扩容检查。

```java
/**
     * Appends the specified element to the end of this list.
     *
     * @param e element to be appended to this list
     * @return <tt>true</tt> (as specified by {@link Collection#add})
     */
    public boolean add(E e) {
        //检查是否需要扩容
        ensureCapacityInternal(size + 1);  // Increments modCount!!
        //把元素插入到最后一位
        elementData[size++] = e;
        return true;
    }

    private void ensureCapacityInternal(int minCapacity) {
        //如果是空数组，则将容量扩充至默认容量（10）
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
            minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity);
        }

        ensureExplicitCapacity(minCapacity);
    }

    private void ensureExplicitCapacity(int minCapacity) {
        modCount++;

        // overflow-conscious code
        //扩容
        if (minCapacity - elementData.length > 0)
            grow(minCapacity);
    }


    **
     * Increases the capacity to ensure that it can hold at least the
     * number of elements specified by the minimum capacity argument.
     *
     * @param minCapacity the desired minimum capacity
     */
    private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        // 新容量为旧容量的1.5倍
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        //// 如果新容量发现比需要的容量还小，则以需要的容量为准
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        // 如果新容量已经超过最大容量了，则使用最大容量
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        // 以新容量拷贝出来一个新数组
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
    private static int hugeCapacity(int minCapacity) {
        if (minCapacity < 0) // overflow
            throw new OutOfMemoryError();
        return (minCapacity > MAX_ARRAY_SIZE) ?
            Integer.MAX_VALUE :
            MAX_ARRAY_SIZE;
    }
```

通过上诉方法分析可知：

- 对于元素添加到集合的末尾，首先要进行扩容检查。

- 对于集合扩容：

  ​       如果原集合是空数组，这容量默认扩展到10

  ​       如果原集合不为空数组，这默认容量扩展到原来的1.5倍，然后在进行判断，扩展后容量还是比需要容量   小，则以需要的容量扩展容量

- 集合扩容得到是一个新的数组，不违背java中关于数组是静态的准则，就是通过扩容以及拷贝至新数组操作实现逻辑上的动态数组的功能。

**add(int index, E element)方法**：

添加元素到指定位置，平均时间复杂度为O(n)。

```java
/**
     * Inserts the specified element at the specified position in this
     * list. Shifts the element currently at that position (if any) and
     * any subsequent elements to the right (adds one to their indices).
     *
     * @param index index at which the specified element is to be inserted
     * @param element element to be inserted
     * @throws IndexOutOfBoundsException {@inheritDoc}
     */
    public void add(int index, E element) {
        //检查是否越界
        rangeCheckForAdd(index);
        // 检查是否需要扩容
        ensureCapacityInternal(size + 1);  // Increments modCount!!
        //将index及之后的元素往后挪一位。具体操作和数组插入一致。
        System.arraycopy(elementData, index, elementData, index + 1,
                         size - index);
        elementData[index] = element;
        //元素个数加一
        size++;
    }


/**
     * A version of rangeCheck used by add and addAll. 
     *检查越界：就是判断插入位置index是否比size大
     *只检查是否越上界，如果越上界抛出IndexOutOfBoundsException异常，如果越下界抛出的是     *ArrayIndexOutOfBoundsException异常。
     */
    private void rangeCheckForAdd(int index) {
        if (index > size || index < 0)
            throw new IndexOutOfBoundsException(outOfBoundsMsg(index));
    }
```

注:对于越界检查。只检查是否越上界，如果越上界抛出IndexOutOfBoundsException异常，如果越下界抛出的是ArrayIndexOutOfBoundsException异常。

**addAll(Collection<? extends E> c)方法**

求两个集合的并集。               

```java
/**
     * Appends all of the elements in the specified collection to the end of
     * this list, in the order that they are returned by the
     * specified collection's Iterator.  The behavior of this operation is
     * undefined if the specified collection is modified while the operation
     * is in progress.  (This implies that the behavior of this call is
     * undefined if the specified collection is this list, and this
     * list is nonempty.)
     *
     * @param c collection containing elements to be added to this list
     * @return <tt>true</tt> if this list changed as a result of the call
     * @throws NullPointerException if the specified collection is null
     */
    public boolean addAll(Collection<? extends E> c) {    //将集合c转化为数组
        Object[] a = c.toArray();
        int numNew = a.length;
        //检查扩容
        ensureCapacityInternal(size + numNew);  // Increments modCount
        //将c中所有元素拷贝之elementData的尾部
        System.arraycopy(a, 0, elementData, size, numNew);
        //元素个数变为2个集合的大小之和
        size += numNew;
        //若集合c为空，添加失败。不为空，合并成功
        return numNew != 0;
    }
```

**get(int index)方法**

获取指定索引位置的元素，时间复杂度为O(1)。

```java
 public E get(int index) {
            //检查索引是否越界，与上面添加指定元素的越界检查一致。
            rangeCheck(index);
            checkForComodification();
            //返回数组index位置的元素
            return ArrayList.this.elementData(offset + index);
        }
```

**remove(int index)方法**

删除指定索引位置的元素，时间复杂度为O(n)。

```java
/**
     * Removes the element at the specified position in this list.
     * Shifts any subsequent elements to the left (subtracts one from their
     * indices).
     *
     * @param index the index of the element to be removed
     * @return the element that was removed from the list
     * @throws IndexOutOfBoundsException {@inheritDoc}
     */
    public E remove(int index) {
        *//越界检查
        rangeCheck(index);

        modCount++;
        //获取index位置的元素
        E oldValue = elementData(index);
        //如果index不是最后一位元素，则将index之后的元素往前挪一位
        int numMoved = size - index - 1;
        if (numMoved > 0)
            System.arraycopy(elementData, index+1, elementData, index,
                             numMoved);
        
        //将最后一个元素上删除，帮助GC清理
        elementData[--size] = null; // clear to let GC do its work
        
        //返回删除的值
        return oldValue;
    }
```

注：*ArrayList删除元素的时候容量没有变化。*

**remove(Object o)方法**

删除指定元素值的元素，时间复杂度为O(n)。

```java
/**
     * Removes the first occurrence of the specified element from this list,
     * if it is present.  If the list does not contain the element, it is
     * unchanged.  More formally, removes the element with the lowest index
     * <tt>i</tt> such that
     * <tt>(o==null&nbsp;?&nbsp;get(i)==null&nbsp;:&nbsp;o.equals(get(i)))</tt>
     * (if such an element exists).  Returns <tt>true</tt> if this list
     * contained the specified element (or equivalently, if this list
     * changed as a result of the call).
     *
     * @param o element to be removed from this list, if present
     * @return <tt>true</tt> if this list contained the specified element
     */
    public boolean remove(Object o) {
        if (o == null) {
            //遍历数组，找到数组中的第一次出现的元素，将其快速删除
            for (int index = 0; index < size; index++)       
                if (elementData[index] == null) {
                    fastRemove(index);
                    return true;
                }
        } else {
            for (int index = 0; index < size; index++)
                if (o.equals(elementData[index])) {
                    fastRemove(index);
                    return true;
                }
        }
        return false;
    }


/*
     * Private remove method that skips bounds checking and does not
     * return the value removed.
     *快速删除，顾名思义，没有越界检查，删除效率更高。
     */
    private void fastRemove(int index) {
        modCount++;
        int numMoved = size - index - 1;
        if (numMoved > 0)
            System.arraycopy(elementData, index+1, elementData, index,
                             numMoved);
        elementData[--size] = null; // clear to let GC do its work
    }
```

注：通过上述方法分析：在查找元素时，若删除元素为null，则使用“==”进行判断，若不为null，则使用equals进行判断（关于==和equals的区别参见我的另外一篇博客：《菜鸟修行之路----Java语言基础：数据类型》）



**retainAll(Collection<?> c)方法**

求两个集合的交集。

```java
/**
     * Retains only the elements in this list that are contained in the
     * specified collection.  In other words, removes from this list all
     * of its elements that are not contained in the specified collection.
     *
     * @param c collection containing elements to be retained in this list
     * @return {@code true} if this list changed as a result of the call
     * @throws ClassCastException if the class of an element of this list
     *         is incompatible with the specified collection
     * (<a href="Collection.html#optional-restrictions">optional</a>)
     * @throws NullPointerException if this list contains a null element and the
     *         specified collection does not permit null elements
     * (<a href="Collection.html#optional-restrictions">optional</a>),
     *         or if the specified collection is null
     * @see Collection#contains(Object)
     */
    public boolean retainAll(Collection<?> c) {
        //判断集合c是否为空
        Objects.requireNonNull(c);
        //调用批量删除方法batchRemove（）,删除不包含在c中的元素
        return batchRemove(c, true);
    }

/**
*批量删除，若complement为true，则删除c中不包含的元素
*若complement为flase，则删除c中包含的元素
*/
    private boolean batchRemove(Collection<?> c, boolean complement) {
        final Object[] elementData = this.elementData;
        // 使用读写两个指针同时遍历数组
    // 读指针每次自增1，写指针放入元素的时候才加1
    // 这样不需要额外的空间，就只需要在原有的数组上进行操作
        int r = 0, w = 0;
        boolean modified = false;
        try {
            for (; r < size; r++)
                if (c.contains(elementData[r]) == complement)
                    elementData[w++] = elementData[r];
        } finally {
            // Preserve behavioral compatibility with AbstractCollection,
            // even if c.contains() throws.
            if (r != size) {
                System.arraycopy(elementData, r,
                                 elementData, w,
                                 size - r);
                w += size - r;
            }
            if (w != size) {
                // clear to let GC do its work
                for (int i = w; i < size; i++)
                    elementData[i] = null;
                modCount += size - w;
                //// 新大小等于写指针的位置（因为每写一次写指针就加1，所以新大小正好等于写指针的位置）
                size = w;
                modified = true;
            }
        }
        return modified;
    }
```

对于批量操作方法（采用的是双指针遍历数组，具体可以实现原理可以参见数据结构之数组操作）；

（1）遍历elementData数组；

（2）如果元素在c中，则把这个元素添加到elementData数组的w位置并将w位置往后移一位；

（3）遍历完之后，w之前的元素都是两者共有的，w之后（包含）的元素不是两者共有的；

（4）将w之后（包含）的元素置为null，方便GC回收；

**removeAll(Collection<?> c)**

求两个集合的单方向差集，只保留当前集合中不在c中的元素，不保留在c中不在当前集合中的元素。

例如：a={1，2，3，4，0}，c={2,4,5,3}.则a和b的单方向差集为{1，0}

```java
public boolean removeAll(Collection<?> c) {
        Objects.requireNonNull(c);
      // 调用批量删除方法，complement传入false，表示删除包含在c中的元素
        return batchRemove(c, false);
    }
```

**通过对于上述核心方法的分析**：

ArrayList内部使用数组存储元素，当数组长度不够时进行扩容，默认增加到原来的1.5倍，在底层创建一个新的数组。ArrayList容量不会减少。

ArrayList支持随机访问，通过索引访问元素极快，时间复杂度为O(1)；

ArrayList添加元素到尾部极快，平均时间复杂度为O(1)；

ArrayList添加元素到中间比较慢，因为要搬移元素，平均时间复杂度为O(n)；

ArrayList从尾部删除元素极快，时间复杂度为O(1)；

ArrayList从中间删除元素比较慢，因为要搬移元素，平均时间复杂度为O(n)；



### 1.3Arraylist的应用

**应用实例一：增删改查**

```java
public class ArrayListTest {
    public static void main(String[] args){
        //创建一个空集合
        List list =new ArrayList();
        //添加元素，在添加了第一个元素后，集合容量默认设置为10
        list.add(1);
        list.add(2);
        list.add(3);
        //删除索引为2的元素
        list.remove(2);
        //获取索引为1的元素，并且将其转化为字符串。
        String mun= list.get(1).toString();
        System.out.println(list);
    }
}
```

**应用实例二：遍历集合**

```java
public class ArrayListTest {
    public static void main(String[] args) {
        
        List<Integer> list = new ArrayList<Integer>();
        Integer n = 10;
        for(int i = 0;i < n;i++) {
            list.add(i);
        }
        //方法一：常规遍历
        for(int j = 0; j <list.size();j++) {
            System.out.print(list.get(j)+" ");
        }
        
        //方法二：iterator 迭代遍历
        for (Iterator iterator = list.iterator(); iterator.hasNext(); ) {
            System.out.print(iterator.next()+" ");
        }
        
        //方法三：增强for循环
        for (int i : list) {
            System.out.print(list.get(i)+" ");
        }
        
        //方法四：forEach
        list.forEach(
            item -> {
                System.out.print(item+" ");
        });
        
        //方法五:stream().forEach
        list.stream().forEach(
            item -> {
                System.out.print(item+" ");
        });
    }
}
```



**后记：修行之路艰辛，与君共勉。**

​                                                                                                                                                             ---2020年春：成都

