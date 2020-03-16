# 菜鸟修行之路----Java集合类四：Map之HashMap源码学习



Map接口概述：

​        Map是由一系列键值对组成的集合，提供了key到Value的映射。同时它也没有继承Collection。在Map中它保证了key与value之间的一一对应关系。也就是说一个key对应一个value，所以它**不能存在相同的key值，当然value值可以相同**。

​       Map的实现类有3个：**HashMap ，hashtable，TreeMap** 

## 1.HashMap概述

​        底层数据结构为：哈希表，查找对象时通过哈希函数计算其位置。

​        它是为**快速查询**而设计的，其内部定义了一个hash表数组（Entry[] table），元素会通过哈希转换函数将元素的哈希地址转换成数组中存放的索引，如果有冲突，则使用散列链表的形式将所有相同哈希地址的元素串起来。

​      HashMap具有以下特点：

- 键不可重复，值可以重复
- 线程不安全，一般用于单线程中
- 允许key和value的值为null
-  底层数据结构为：**哈希表**

## 2.继承关系图

![1583374074181](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1583374074181.png)

通过继承关系图可以得知：

- 实现了Cloneable接口，可以被克隆。
- 实现了Serializable接口，可以被序列化。
- 继承自AbstractMap类，实现了Map接口，具有Map的所有功能。

## 3.存储结构

​        HashMap的实现采用了（数组 + 链表 + 红黑树）的复杂结构，数组的一个元素又称作桶。

​        在添加元素时，会根据hash值算出元素在数组中的位置，如果该位置没有元素，则直接把元素放置在此处，如果该位置有元素了，则把元素以链表的形式放置在链表的尾部。

​       当一个链表的元素个数达到一定的数量（且数组的长度达到一定的长度：小于6转化为链表，大于8转化为红黑树）后，则把链表转化为红黑树，从而提高效率。

​       数组的查询效率为O(1)，链表的查询效率是O(k)，红黑树的查询效率是O(log k)，k为桶中的元素个数，所以当元素数量非常多的时候，转化为红黑树能极大地提高效率。

​      具体如下图所示：

​     ![1583374507639](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1583374507639.png)

## 4.数据结构基础：散列表（HashTable）

提供key和value的映射关系，查找时间复杂度接近为O（1）。

散列表本质上是一个数组，通过哈希函数实现key值与数组下标进行互换。在java语言中，每一个对象都有一个属于自己的hashcode（整型变量），通过取模运算实现互换。

哈希函数：

```java
index=HashCode(Key)%Array.length
```

注：在JDK中，采用**位运算**来转换，优化性能

**散列表的写操作（put）**

将新键值对（key1，Value1）写入散列表步骤：（在JDK中用Entry对象来描述键值对）

- 通过哈希函数，将key1转化为数组下标index1
- 判断index1位置是否存在元素：

​             如果没有就插入元素。

​             如果已经存在元素，则发生哈希冲突（常用解决方式：开发寻址法，链表法（HashMap中采用的就是链表法解决哈希冲突））



**HashMap中的哈希冲突的解决**

将数组元素作为该链表的头节点，后面插入进来的元素（相同下标）依次插入到该链表中。

**注：在HashMap中如果链表中元素个数大于8个时，会将其转化为红黑树**（**数组元素个数大于64才能进行树化**）

**在JDK 8以上版本才支持转化为红黑树的**  

**散列表的读操作（get）**

通过给定的key值，获取对应的value值，步骤与写操作类似。

首先利用哈希函数将key转化为对应的index（数组下标）；

接着查找数组，如果数组下标index对应的元素的key与查找的不一致，就继续查找该index对应的链表。



**散列表的扩容（resize）**

散列表是基于数组实现的，所以也存在扩容操作。还是以HashMap进行分析。

扩容要求:**当hashMap中的元素个数大于等于容量（数组的当前长度）*装载因子（默认为0.75）**
$$
HashMap.Size>=Capacity*LoadFactor
$$
扩容的具体操作：

扩容，创建一个新的Entry数组，长度为原数组的2倍

重建Hash表，遍历原来的Entry数组，将其中的元素使用哈希函数重新计算下标，然后在将其写入到新数组中



通过上诉数据结构基础的学习，就比较好理解下面复制的HashMap源码了。

## 5.源码分析

### 5.1 成员变量

```java
/**
     * The default initial capacity - MUST be a power of two.
     */
    //默认初始容量为16
    static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16

    /**
     * The maximum capacity, used if a higher value is implicitly specified
     * by either of the constructors with arguments.
     * MUST be a power of two <= 1<<30.
     */
     //最大容量为2的30次方
    static final int MAXIMUM_CAPACITY = 1 << 30;

    /**
     * The load factor used when none specified in constructor.
     */
     //默认的装载因子
    static final float DEFAULT_LOAD_FACTOR = 0.75f;

    /**
     * The bin count threshold for using a tree rather than list for a
     * bin.  Bins are converted to trees when adding an element to a
     * bin with at least this many nodes. The value must be greater
     * than 2 and should be at least 8 to mesh with assumptions in
     * tree removal about conversion back to plain bins upon
     * shrinkage.
     */
    //当一个桶中的元素个数大于等于8时进行树化，转化为红黑树
    static final int TREEIFY_THRESHOLD = 8;

    /**
     * The bin count threshold for untreeifying a (split) bin during a
     * resize operation. Should be less than TREEIFY_THRESHOLD, and at
     * most 6 to mesh with shrinkage detection under removal.
     */
    //当一个桶中的元素个数小于等于6时把树转化为链表
    static final int UNTREEIFY_THRESHOLD = 6;

    /**
     * The smallest table capacity for which bins may be treeified.
     * (Otherwise the table is resized if too many nodes in a bin.)
     * Should be at least 4 * TREEIFY_THRESHOLD to avoid conflicts
     * between resizing and treeification thresholds.
     */
     //当桶的个数达到64的时候才进行树化
    static final int MIN_TREEIFY_CAPACITY = 64;
/**
     * The table, initialized on first use, and resized as
     * necessary. When allocated, length is always a power of two.
     * (We also tolerate length zero in some operations to allow
     * bootstrapping mechanics that are currently not needed.)
     */
    //数组，又叫作桶（bucket）
    transient Node<K,V>[] table;

    /**
     * Holds cached entrySet(). Note that AbstractMap fields are used
     * for keySet() and values().
     */
    //作为entrySet()的缓存
    transient Set<Map.Entry<K,V>> entrySet;

    /**
     * The number of key-value mappings contained in this map.
     */
    ///元素个数
    transient int size;

    /**
     * The number of times this HashMap has been structurally modified
     * Structural modifications are those that change the number of mappings in
     * the HashMap or otherwise modify its internal structure (e.g.,
     * rehash).  This field is used to make iterators on Collection-views of
     * the HashMap fail-fast.  (See ConcurrentModificationException).
     */
    //修改次数，用于在迭代的时候执行快速失败策略
    transient int modCount;

    /**
     * The next size value at which to resize (capacity * load factor).
     *
     * @serial
     */
    // (The javadoc description is true upon serialization.
    // Additionally, if the table array has not been allocated, this
    // field holds the initial array capacity, or zero signifying
    // DEFAULT_INITIAL_CAPACITY.)
    //当桶的使用数量达到多少时进行扩容，threshold = capacity * loadFactor（容量*装载因子（默认设置为0.75））
    int threshold;

    /**
     * The load factor for the hash table.
     *
     * @serial
     */
    //装载因子 
    final float loadFactor;
```

通过上诉源码可以得知：

（1）容量为数组的长度，亦即桶（数组的一个元素为一个桶）的个数，默认为16，最大为2的30次方，当容量达到64时才可以树化。

（2）装载因子用来计算容量达到多少时才进行扩容，默认装载因子为0.75。

（3）树化，当容量达到64且链表的长度达到8时进行树化，当链表的长度小于6时反树化。

### 5.2 构造方法

**HashMap()构造方法**

空参构造方法，全部使用默认值（容量为16，装载因子为0.75）。

```java
/**
     * Constructs an empty <tt>HashMap</tt> with the default initial capacity
     * (16) and the default load factor (0.75).
     */
    public HashMap() {
        this.loadFactor = DEFAULT_LOAD_FACTOR; // all other fields defaulted
    }
```

**HashMap(int initialCapacity)构造方法**

使用自定义初始容量（外部传入）和默认装载因子（0.75）。

```java
/**
     * Constructs an empty <tt>HashMap</tt> with the specified initial
     * capacity and the default load factor (0.75).
     *
     * @param  initialCapacity the initial capacity.
     * @throws IllegalArgumentException if the initial capacity is negative.
     */
    public HashMap(int initialCapacity) {
        this(initialCapacity, DEFAULT_LOAD_FACTOR);
    }
```

**HashMap(int initialCapacity)构造方法**

判断传入的初始容量和装载因子是否合法，并计算扩容门槛，扩容门槛为传入的初始容量往上取最近的2的n次方。

```java
/**
     * Constructs an empty <tt>HashMap</tt> with the specified initial
     * capacity and load factor.
     *
     * @param  initialCapacity the initial capacity
     * @param  loadFactor      the load factor
     * @throws IllegalArgumentException if the initial capacity is negative
     *         or the load factor is nonpositive
     */
    public HashMap(int initialCapacity, float loadFactor) {
        //检测传入的初始容量是否合法（是否大于0，是否大于最大值）
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal initial capacity: " +
                                               initialCapacity);
        if (initialCapacity > MAXIMUM_CAPACITY)
            initialCapacity = MAXIMUM_CAPACITY;
        //装载因子是否合法
        if (loadFactor <= 0 || Float.isNaN(loadFactor))
            throw new IllegalArgumentException("Illegal load factor: " +
                                               loadFactor);
        this.loadFactor = loadFactor;
        // 计算扩容门槛
        this.threshold = tableSizeFor(initialCapacity);
    }


/**
     * Returns a power of two size for the given target capacity.
     */
    static final int tableSizeFor(int cap) {
        // 扩容门槛为传入的初始容量往上取最近的2的n次方
        int n = cap - 1;
        n |= n >>> 1;
        n |= n >>> 2;
        n |= n >>> 4;
        n |= n >>> 8;
        n |= n >>> 16;
        return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
    }
```

### 5.3内部类

**Node内部类**

Node是一个典型的**单链表**节点，其中，**hash用来存储key计算得来的hash值。**

```java
/**
     * Basic hash bin node, used for most entries.  (See below for
     * TreeNode subclass, and in LinkedHashMap for its Entry subclass.)
     */
    static class Node<K,V> implements Map.Entry<K,V> {
        final int hash;
        final K key;
        V value;
        Node<K,V> next;
       //构造方法
        Node(int hash, K key, V value, Node<K,V> next) {
            this.hash = hash;
            this.key = key;
            this.value = value;
            this.next = next;
        }
        //获取值
        public final K getKey()        { return key; }
        public final V getValue()      { return value; }
        public final String toString() { return key + "=" + value; }

        public final int hashCode() {
            return Objects.hashCode(key) ^ Objects.hashCode(value);
        }

        public final V setValue(V newValue) {
            V oldValue = value;
            value = newValue;
            return oldValue;
        }
 
        public final boolean equals(Object o) {
            if (o == this)
                return true;
            if (o instanceof Map.Entry) {
                Map.Entry<?,?> e = (Map.Entry<?,?>)o;
                if (Objects.equals(key, e.getKey()) &&
                    Objects.equals(value, e.getValue()))
                    return true;
            }
            return false;
        }
    }
```

**TreeNode内部类**

它继承自LinkedHashMap中的Entry类，TreeNode是一个典型的树型节点，其中，prev是链表中的节点，用于在删除元素的时候可以快速找到它的前置节点。

```java
// 位于HashMap中
static final class TreeNode<K,V> extends LinkedHashMap.Entry<K,V> {
    TreeNode<K,V> parent;  // red-black tree links
    TreeNode<K,V> left;
    TreeNode<K,V> right;
    TreeNode<K,V> prev;    // needed to unlink next upon deletion
    boolean red;
}
 
// 位于LinkedHashMap中，典型的双向链表节点
static class Entry<K,V> extends HashMap.Node<K,V> {
    Entry<K,V> before, after;
    Entry(int hash, K key, V value, Node<K,V> next) {
        super(hash, key, value, next);
    }
}
```

### 5.4核心方法

**常用方法总结**

| 方法                        | 功能                                                         |
| --------------------------- | ------------------------------------------------------------ |
| put(K key, V value)         | 将键（key）/值（value）映射存放到Map集合中                   |
| get(Object key)             | 返回指定键所映射的值，没有该key对应的值则返回 null。         |
| size()                      | 返回Map集合中数据数量                                        |
| clear()                     | 清空Map集合                                                  |
| isEmpty ()                  | 判断Map集合中是否有数据，如果没有则返回true，否则返回false   |
| remove(Object key)          | 删除Map集合中键为key的数据并返回其所对应value值。            |
| values()                    | 返回Map集合中所有value组成的以Collection数据类型格式数据。   |
| keySet()                    | 返回Map集合中所有key组成的Set集合                            |
| containsKey(Object key)     | 判断集合中是否包含指定键，包含返回 true，否则返回false       |
| containsValue(Object value) | 判断集合中是否包含指定值，包含返回 true，否则返回false。     |
| entrySet()                  | Map集合每个key-value转换为一个Entry对象并返回由所有的Entry对象组成的Set集合，遍历HashMap |

**重要方法的源码分析：**

**put(K key, V value)方法**

添加元素的入口。

```java
/**
     * Associates the specified value with the specified key in this map.
     * If the map previously contained a mapping for the key, the old
     * value is replaced.
     *
     * @param key key with which the specified value is to be associated
     * @param value value to be associated with the specified key
     * @return the previous value associated with <tt>key</tt>, or
     *         <tt>null</tt> if there was no mapping for <tt>key</tt>.
     *         (A <tt>null</tt> return can also indicate that the map
     *         previously associated <tt>null</tt> with <tt>key</tt>.)
     */
    public V put(K key, V value) {
        //调用hash(key)计算出key的hash值，将其作为参数传给putVal
        return putVal(hash(key), key, value, false, true);
    }

    static final int hash(Object key) {
        int h;
        // 如果key为null，则hash值为0，否则调用key的hashCode()方法，获取hash值
        // 并让高16位与整个hash异或，这样做是为了使计算出的hash更分散
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }


 /**
     * Implements Map.put and related methods
     *
     * @param hash hash for key
     * @param key the key
     * @param value the value to put
     * @param onlyIfAbsent if true, don't change existing value
     * @param evict if false, the table is in creation mode.
     * @return previous value, or null if none
     */
    final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
               boolean evict) {
    Node<K, V>[] tab;
    Node<K, V> p;
    int n, i;
    // 如果桶的数量为0，则初始化
    if ((tab = table) == null || (n = tab.length) == 0)
        // 调用resize()初始化
        n = (tab = resize()).length;
    // (n - 1) & hash 计算元素在哪个桶中
    // 如果这个桶中还没有元素，则把这个元素放在桶中的第一个位置
    if ((p = tab[i = (n - 1) & hash]) == null)
        // 新建一个节点放在桶中
        tab[i] = newNode(hash, key, value, null);
    else {
        // 如果桶中已经有元素存在了
        Node<K, V> e;
        K k;
        // 如果桶中第一个元素的key与待插入元素的key相同，保存到e中用于后续修改value值
        if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
            e = p;
        else if (p instanceof TreeNode)
            // 如果第一个元素是树节点，则调用树节点的putTreeVal插入元素
            e = ((TreeNode<K, V>) p).putTreeVal(this, tab, hash, key, value);
        else {
            // 遍历这个桶对应的链表，binCount用于存储链表中元素的个数
            for (int binCount = 0; ; ++binCount) {
                // 如果链表遍历完了都没有找到相同key的元素，说明该key对应的元素不存在，则在链表最后插入一个新节点
                if ((e = p.next) == null) {
                    p.next = newNode(hash, key, value, null);
                    // 如果插入新节点后链表长度大于8，则判断是否需要树化，因为第一个元素没有加到binCount中，所以这里-1
                    if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                        treeifyBin(tab, hash);
                    break;
                }
                // 如果待插入的key在链表中找到了，则退出循环
                if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                    break;
                p = e;
            }
        }
        // 如果找到了对应key的元素
        if (e != null) { // existing mapping for key
            // 记录下旧值
            V oldValue = e.value;
            // 判断是否需要替换旧值
            if (!onlyIfAbsent || oldValue == null)
                // 替换旧值为新值
                e.value = value;
            // 在节点被访问后做点什么事，在LinkedHashMap中用到
            afterNodeAccess(e);
            // 返回旧值
            return oldValue;
        }
    }
    // 到这里了说明没有找到元素
    // 修改次数加1
    ++modCount;
    // 元素数量加1，判断是否需要扩容
    if (++size > threshold)
        // 扩容
        resize();
    afterNodeInsertion(evict);
    // 没找到元素返回null
    return null;
}
```

通过上诉源码分析可以得知，元素添加步骤如下：

- 计算key的hash值；

- 如果桶（数组）数量为0，则初始化桶；

- 如果key所在的桶没有元素，则直接插入；

- 如果key所在的桶中的第一个元素的key与待插入的key相同，说明找到了元素，则判断是否需要替换旧值，并直接返回旧值；

- 如果第一个元素是树节点，则调用树节点的putTreeVal()寻找元素或插入树节点；

- 如果不是以上三种情况，则遍历桶对应的链表查找key是否存在于链表中：

  ​        如果找到了对应key的元素，则判断是否需要替换旧值，并直接返回旧值；

  ​        如果没找到对应key的元素，则在链表最后插入一个新节点并判断是否需要树化；

- 如果插入了元素，则数量加1并判断是否需要扩容；

**resize()方法**

扩容方法：默认将数组长度扩容到之前的2倍。

```java
final Node<K, V>[] resize() {
    // 旧数组
    Node<K, V>[] oldTab = table;
    // 旧容量
    int oldCap = (oldTab == null) ? 0 : oldTab.length;
    // 旧扩容门槛
    int oldThr = threshold;
    int newCap, newThr = 0;
    if (oldCap > 0) {
        if (oldCap >= MAXIMUM_CAPACITY) {
            // 如果旧容量达到了最大容量，则不再进行扩容
            threshold = Integer.MAX_VALUE;
            return oldTab;
        } else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                oldCap >= DEFAULT_INITIAL_CAPACITY)
            // 如果旧容量的两倍小于最大容量并且旧容量大于默认初始容量（16），则容量扩大为两部，扩容门槛也扩大为两倍
            newThr = oldThr << 1; // double threshold
    } else if (oldThr > 0) // initial capacity was placed in threshold
        // 使用非默认构造方法创建的map，第一次插入元素会走到这里
        // 如果旧容量为0且旧扩容门槛大于0，则把新容量赋值为旧门槛
        newCap = oldThr;
    else {               // zero initial threshold signifies using defaults
        // 调用默认构造方法创建的map，第一次插入元素会走到这里
        // 如果旧容量旧扩容门槛都是0，说明还未初始化过，则初始化容量为默认容量，扩容门槛为默认容量*默认装载因子
        newCap = DEFAULT_INITIAL_CAPACITY;
        newThr = (int) (DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
    }
    if (newThr == 0) {
        // 如果新扩容门槛为0，则计算为容量*装载因子，但不能超过最大容量
        float ft = (float) newCap * loadFactor;
        newThr = (newCap < MAXIMUM_CAPACITY && ft < (float) MAXIMUM_CAPACITY ?
                (int) ft : Integer.MAX_VALUE);
    }
    // 赋值扩容门槛为新门槛
    threshold = newThr;
    // 新建一个新容量的数组
    @SuppressWarnings({"rawtypes", "unchecked"})
    Node<K, V>[] newTab = (Node<K, V>[]) new Node[newCap];
    // 把桶赋值为新数组
    table = newTab;
    // 如果旧数组不为空，则搬移元素
    if (oldTab != null) {
        // 遍历旧数组
        for (int j = 0; j < oldCap; ++j) {
            Node<K, V> e;
            // 如果桶中第一个元素不为空，赋值给e
            if ((e = oldTab[j]) != null) {
                // 清空旧桶，便于GC回收  
                oldTab[j] = null;
                // 如果这个桶中只有一个元素，则计算它在新桶中的位置并把它搬移到新桶中
                // 因为每次都扩容两倍，所以这里的第一个元素搬移到新桶的时候新桶肯定还没有元素
                if (e.next == null)
                    newTab[e.hash & (newCap - 1)] = e;
                else if (e instanceof TreeNode)
                    // 如果第一个元素是树节点，则把这颗树打散成两颗树插入到新桶中去
                    ((TreeNode<K, V>) e).split(this, newTab, j, oldCap);
                else { // preserve order
                    // 如果这个链表不止一个元素且不是一颗树
                    // 则分化成两个链表插入到新的桶中去
                    // 比如，假如原来容量为4，3、7、11、15这四个元素都在三号桶中
                    // 现在扩容到8，则3和11还是在三号桶，7和15要搬移到七号桶中去
                    // 也就是分化成了两个链表
                    Node<K, V> loHead = null, loTail = null;
                    Node<K, V> hiHead = null, hiTail = null;
                    Node<K, V> next;
                    do {
                        next = e.next;
                        // (e.hash & oldCap) == 0的元素放在低位链表中
                        // 比如，3 & 4 == 0
                        if ((e.hash & oldCap) == 0) {
                            if (loTail == null)
                                loHead = e;
                            else
                                loTail.next = e;
                            loTail = e;
                        } else {
                            // (e.hash & oldCap) != 0的元素放在高位链表中
                            // 比如，7 & 4 != 0
                            if (hiTail == null)
                                hiHead = e;
                            else
                                hiTail.next = e;
                            hiTail = e;
                        }
                    } while ((e = next) != null);
                    // 遍历完成分化成两个链表了
                    // 低位链表在新桶中的位置与旧桶一样（即3和11还在三号桶中）
                    if (loTail != null) {
                        loTail.next = null;
                        newTab[j] = loHead;
                    }
                    // 高位链表在新桶中的位置正好是原来的位置加上旧容量（即7和15搬移到七号桶了）
                    if (hiTail != null) {
                        hiTail.next = null;
                        newTab[j + oldCap] = hiHead;
                    }
                }
            }
        }
    }
    return newTab;
}
```

通过上诉源码分析可以得知：

- 如果使用是默认构造方法，则第一次插入元素时初始化为默认值，容量为16，扩容门槛为12；
- 如果使用的是非默认构造方法，则第一次插入元素时初始化容量等于扩容门槛，扩容门槛在构造方法里等于传入容量向上最近的2的n次方；
- 如果旧容量大于0，则新容量等于旧容量的2倍，但不超过最大容量2的30次方，新扩容门槛为旧扩容门槛的2倍；
- 创建一个新容量的桶；
- 搬移元素，原链表分化成两个链表，低位链表存储在原来桶的位置，高位链表搬移到原来桶的位置加旧容量的位置；



**注：后续转化为红黑树以及插入元素到红黑树中，遍历红黑树等等，放在树的学习之后继续。**





## 6.总结

（1）HashMap是一种散列表，采用（数组 + 链表 + 红黑树）的存储结构；

（2）HashMap的默认初始容量为16（1<<4），默认装载因子为0.75f，容量总是2的n次方；

（3）HashMap扩容时每次容量变为原来的两倍；

（4）当桶的数量小于64时不会进行树化，只会扩容；

（5）当桶的数量大于64且单个桶中元素的数量大于8时，进行树化；

（6）当单个桶中元素数量小于6时，进行反树化；

（7）HashMap是非线程安全的容器；

（8）HashMap查找添加元素的时间复杂度都为O(1)；

## 7.HashMap的应用

**典型应用一：增删改查**

```java
public class HashMapTest {
    public static void main(String[ ] args){

        HashMap hashMap=new HashMap();
        //插入元素
        hashMap.put(1,"001");
        hashMap.put(2,"002");
        //删除元素
        hashMap.remove(1);
        //获取元素，转化为字符串
        String num=hashMap.get(2).toString();
    }
}
```

**典型应用二：遍历HashMap**

```java
public class HashMapTest {
    public static void main(String[ ] args){

        Map<Integer, String> map = new HashMap<>();
        map.put(1, "a");
        map.put(3, "c");
        map.put(5, "e");
        map.put(2, "b");
        //通过forEach循环遍历
        for (Map.Entry<Integer, String> entry : map.entrySet()) {
            System.out.println("Key = " + entry.getKey() + " Value = "+ entry.getValue());
        }
        //通过forEach迭代键值对
        for (Integer key : map.keySet()) {
            System.out.println("Key = " + key);
        }
        for (String value : map.values()) {
            System.out.println("Value = " + value);
        }
        //使用迭代器进行遍历
        Iterator<Map.Entry<Integer, String>> iterator = map.entrySet().iterator();
        while (iterator.hasNext()) {
            Map.Entry<Integer, String> entry = (Map.Entry<Integer, String>)iterator.next();
            Integer key = entry.getKey();
            String value = entry.getValue();
            System.out.println("Key = " + key + " Value = "+ value);
        }
        //通过Java8 Lambda表达式遍历
        map.forEach((key, value) -> {
            System.out.println("Key = " + key + "  " + " Value = " + value);
        });
    }
}
```

**后记**：

通过一周的学习，完成对于线性表相关知识以及对应典型实例的学习

- **ArrayList---->>数组**
- **LinkedList--->>链表（双向循环链表）---->>队列---->>栈**
- **HashMap--->>数组+链表（单链表）+红黑树（下一阶段）**

通过对上诉3种典型集合的源码学习，继续加深了对于基础数据结构的理解。

这个也是本人的第8篇博客，修行之路艰辛，与君共勉。

​                                                                                           ----2020年3月 成都