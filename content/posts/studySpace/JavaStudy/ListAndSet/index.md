---
date: '2025-06-04T18:06:21+08:00'
draft: false
title: '集合'
seriesOpened: true #s是否开启系列
series: ["java学习笔记"] #属于的系列 
series_order: 10  #系列编号
showSummary: true #摘要信息
summary: "" #摘要信息
tags: ["Java基础"]
Categories: ["Java","学习笔记"]
layoutBackgroundBlur: true #向下滚动主页时，是否模糊背景图。
layoutBackgroundHeaderSpace: true #在标题和正文之间添加空白区域间隔。
---


用于存储、操作和传输数据的容器，提供了丰富的接口和实现类，分为 **单列集合（Collection）** 和 **双列集合（Map）** 两大类。

## 单列集合

{{<alert>}}
**Collection** 是单列集合的顶部接口。
{{</alert>}}

该接口主要包含以下方法
<!--
|                             | 方法名                           | 功能 |
| ------------------------------- | -------------------------------- |
| `boolean add(E e)`              | 添加元素，返回是否添加成功。     |
| `boolean remove(Object obj);`   | 根据元素删除，返回是否删除成功。 |
| `boolean contains(Object obj);` | 判断集合中是否包含某一个元素。   |
| `int size();`                   | 求集合的元素个数。               |  |
--> 
~~~Java
public interface Collection<E> extends Iterable<E> {
    //基础操作
    boolean add(E e);          // 添加元素
    boolean remove(Object o);  // 删除元素
    void clear();             // 清空集合
    int size();               // 元素数量
    boolean isEmpty();        // 是否为空

    //包含判断
    boolean contains(Object o);

    //批量操作
    boolean addAll(Collection<? extends E> c);  // 添加另一个集合所有元素
    boolean removeAll(Collection<?> c);        // 删除与另一集合共有的元素
    boolean retainAll(Collection<?> c);        // 仅保留与另一集合共有的元素

    //遍历操作
    Iterator<E> iterator();  // 获取迭代器
}
~~~

### List集合

**ArrayList**

~~~java
ArrayList<Type> listName = new ArrayList<>();
~~~

主要特点：
1. 有序、允许重复元素、非线程安全、基于索引的随机访问效率高。
2. 查改快——基于索引直接定位，O(1)时间复杂度。
3. 增删慢——中间操作要挪数据，O(n)时间复杂度。

底层采用数组实现，默认初始容量 10，扩容 1.5 倍。

**LinkedList**

~~~java
LinkedList<Type> listName = new LinkedList<>();
~~~

主要特点：
1. 有序、允许重复元素、非线程安全。
2. 插入/删除快（`O(1)`）。
3. 随机访问慢（需遍历，`O(n)`）。

底层采用双向链表，并维护了头节点和尾节点，在插入删除操作越靠近中间越慢，因为需要先检索到中间的位置。

{{<alert>}}
但是在数据量庞大，在中间插入元素的场景下Arraylist比LinkedList更快。
{{</alert>}}

> ArrayList 比 LinkedList 快的原因是有三个原因  
> 1. 第一是因为内存局部性原理， ArrayList 在连续内存中，能一次将一定大小的元素加载到 CPU 缓存里， LinkedList 分散在内存不同的地方，缓存命中率低。  
> 
> 2. 第二是对于大数据量，内存连续拷贝比遍历链表更快， LinkedList 主要耗时花在了内存分配和指针操作。  
> 
> 3. 第三是 JVM 针对 ArrayList 进行了优化。  

### 迭代器

`Iterator` 接口是 Java 集合框架中的一个重要接口，它提供了一种统一的方式来遍历各种集合中的元素，而不需要了解集合的内部结构。

~~~Java
public interface Iterator<E> {
    boolean hasNext();  // 检查是否还有下一个元素
    E next();          // 获取下一个元素
    void remove();     // 移除当前元素(可选操作)
}
~~~

**Iterable 接口**

如果一个类实现了这个接口，那么表示这个类是可迭代的。

~~~java
public interface Iterable<T> {
    Iterator<T> iterator();  // 核心方法，返回迭代器
    // Java 8 新增的默认方法...
}
~~~

这个接口会返回一个迭代器，这个迭代器必须在类的内部进行声明并继承 `Iterator` 接口，一般是声明为内部类的形式。

~~~java
@Override  //重写 Iterable 接口下的方法
public Iterator<E> iterator() {
    return new Itr();
}

//声明一个内部类
private class Itr implements Iterator<E> {
    int cursor;
    int expectedModCount = modCount;

    @Override
    public boolean hasNext() {
        return cursor != size;
    }

    @Override
    public E next() {
        return (E) element;
    }

    private void checkModification() {
        if (modCount != expectedModCount) {
            throw new RuntimeException("集合已经被不可预料的修改");
        }
    }
}
~~~

**迭代器使用**

~~~java
Iterator<Integer> it = list.iterator();
while (it.hasNext()) {
    Integer num = it.next();
    sum += num;
}
~~~

> Q：迭代器内部是怎么索引到下一个对象的？  
> A：内部维护一个 `cursor` 指针, 当我们调用 `hasNext()` 方法的时候，根据 `cursor` 是否越界返回一个 `boolean` ，如果为 `true` 使用 `next()` 方法获取元素, 先取出 `cursor` 指向的元素，然后 `cursor` 自增 1。如果迭代的是 `LinkedList` ，则内部维护了一个 `next` 节点指向当前元素， `nextIndex` 保存当前元素的下标，类似数组中的下标。
>
> Q：`ConcurrentModificationException`（并发修改异常）是什么，为什么会触发？    
> A：这个异常时因为在遍历时集合内部的 `modCount` 和迭代器中的 `expectedModCount` 不一致导致的错误， `modCount` 记录当前集合的修改次数，在创建迭代器时，会将 `expectedModCount` 赋值为 `modCount` 的值，如果接下来还会有对集合的操作，则需要使用迭代器中的操作，
>
> Q：为什么迭代器中需要记录 `expectedModCount` ？
> A：如果使用集合类直接删除其中的某一项数据，那么集合后面的元素会进行迁移，导致原本当前元素的索引指向了下一位。导致读取数据少了一条。所以在调用 `next()` 方法时，会先 check 两个值是否相等，如果不一致，则抛出异常

### Set 集合

用于存储 **唯一元素（不重复）** 的接口，其核心实现类（ `HashSet` 、 `LinkedHashSet` 、 `TreeSet` ）底层分别依赖不同的数据结构来保证元素的唯一性和特定顺序。

#### HashSet（无序，基于哈希表）

`HashSet` 是基于哈希表(Hash Table)实现的

底层实现：
- 基于 `HashMap：`  
  `HashSet` 实际上是用 `HashMap` 的 `key` 来存储元素（value用一个固定Object对象填充）。
- 数组+链表/红黑树：  
  主结构是一个 `Node<K,V>[]` 数组（哈希桶数组） 。 
  每个数组元素可能是一个链表或红黑树。
  链表长度 >8 时转为红黑树，红黑树节点数 <6 时转回链表

唯一性基于 `hashcode() `，添加元素时，先计算哈希值定位桶位置。如果哈希冲突，再调用 `equals()` 比较是否重复。，查询/插入平均 O(1)（哈希冲突少时）。**无序**：遍历顺序与插入顺序无关。

**扩容策略**：元素个数 / hash表的长度 > 加载因子(默认为0.75),则会扩容, "二倍"扩容, 所有的元素会重新计算索引重新排列。

~~~java
HashSet<Integer> hs =new HashSet<>();
~~~

#### LinkedHashSet（有序，基于链表+哈希表）

`LinkedHashSet` 是 `HashSet` 的子类，在 `HashMap` 基础上，通过自身内部维护的双向链表维护插入顺序。

特点：
1. 有序：维护插入顺序（或访问顺序）
2. 性能略低：比HashSet稍慢，因为要维护链表
3. 迭代效率高：直接遍历链表即可

#### TreeSet

底层采用红黑树实现，排序规则按照自然排序或自定义排序。

**唯一性保证**
依赖 Comparable 或 Comparator：
如果元素实现 Comparable，按 compareTo() 排序。
可通过构造函数传入 Comparator 自定义排序规则。

**自然排序**

让元素实现Comparable接口, 重写compareTo方法, this代表要添加的元素, o代表集合的元素。

**比较器排序**

创建Comparator对象, 实现compare方法, 里面有两个参数, 第一个参数是要添加的元素, 第二个参数是集合中的元素。

红黑树