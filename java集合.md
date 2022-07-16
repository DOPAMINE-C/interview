# 一.集合概述

Java 集合， 也叫作容器，主要是由两大接口派生而来：一个是 `Collection`接口，**主要用于存放单一元素**；另一个是 `Map` 接口，**主要用于存放键值对**。对于`Collection` 接口，下面又有三个主要的子接口：`List`、`Set` 和 `Queue`。

Java 集合框架如下图所示：

<img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220421151424673.png" alt="image-20220421151424673" style="zoom:50%;" />



## 说说 List, Set, Queue, Map 四者的区别？

- `List`(对付顺序的好帮手): 存储的元素是**有序**的、**可重复**的。按对象进入的顺序保存对象，允许多个Null值，**可以用 Iterator 取出所有元素，再逐一遍历，也可以用get(int index)获取指定下标的元素。**
- `Set`(注重独一无二的性质): 存储的元素是**无序**的、**不可重复**的。最多允许有⼀个Null元素对象，**取元素时只能⽤Iterator接⼝取得所有元 素，再逐⼀遍历各个元素** 
- `Queue`(实现排队功能的叫号机): 按特定的排队规则来确定先后顺序，存储的元素是有序的、可重复的。
- `Map`(用 key 来搜索的专家): 使用键值对（key-value）存储，**key 是无序的、不可重复的**，**value 是无序的、可重复的，每个键最多映射到一个值**。



## 集合框架底层数据结构总结

先来看一下 `Collection` 接口下面的集合。

### List

- `Arraylist`： `Object[]` 数组
- `Vector`：`Object[]` 数组
- `LinkedList`： 双向链表

### Set

- `HashSet`(无序，唯一): 基于 `HashMap` 实现的，底层采用 `HashMap` 来保存元素
- `LinkedHashSet`: 是 `HashSet` 的子类，并且其内部是通过 `LinkedHashMap` 来实现的。有点类似于我们之前说的 `LinkedHashMap` 其内部是基于 `HashMap` 实现一样，不过还是有一点点区别的
- `TreeSet`(***有序***，唯一): 红黑树(自平衡的排序二叉树)

### Queue

- `PriorityQueue`: `Object[]` 数组来实现二叉堆
- `ArrayQueue`: `Object[]` 数组 + 双指针



再来看看 `Map` 接口下面的集合。

### Map

- `HashMap`：数组和单链表的结合体

  1. **底层是数组**。数组的每一个元素是一个单向链表。而单链表的每个节点是键值对类型的。
  2. `JDK1.8 `之前 `HashMap` 由数组+链表组成的，数组是 `HashMap` 的主体，链表则是主要为了解决哈希冲突而存在的（“拉链法”解决冲突）。`JDK1.8` 以后在 *解决哈希冲突* 时有了较大的变化，当链表长度大于阈值（默认为 8）**（将链表转换成红黑树前会判断，如果当前数组的长度小于 64，那么会选择先进行数组扩容，而不是转换为红黑树）**时，将链表转化为红黑树，以减少搜索时间

  ​	其中：`拉链法` 的实现比较简单，将链表和数组相结合。也就是说创建一个**链表数组，数组中每一格就是一个链表**。若遇到哈希冲突，则将冲突的值加到链表中即可:

  ![image-20220421152623417](C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220421152623417.png)

- `LinkedHashMap`：**LinkedHashMap 是根据插入或访问顺序实现有序输出的`HashMap`。** `LinkedHashMap` 继承自 `HashMap`，所以它的底层仍然是基于拉链式散列结构即由数组和链表或红黑树组成。另外，`LinkedHashMap` 在上面结构的基础上，增加了一条双向链表，使得上面的结构可以保持键值对的插入顺序。同时通过对链表进行相应的操作，实现了访问顺序相关逻辑。详细可以查看：[《LinkedHashMap 源码详细分析（JDK1.8）》](https://www.imooc.com/article/22931)

- `Hashtable`： 数组+链表组成的，数组是 `Hashtable` 的主体，链表则是主要为了解决哈希冲突而存在的

- `TreeMap`： **有序**，红黑树（自平衡的排序二叉树）



##  如何选用集合？以及为何要使用集合？

1. 主要根据集合的特点来选用，比如我们需要根据键值获取到元素值时就选用 `Map` 接口下的集合，**需要排序时选择 `TreeMap`**,不需要排序时就选择 `HashMap`,需要保证线程安全就选用 `ConcurrentHashMap`。

   ​	当我们只需要存放元素值时，就选择实现`Collection` 接口的集合，需要保证元素唯一时选择实现 `Set` 接口的集合比如 `TreeSet` 或 `HashSet`，不需要就选择实现 `List` 接口的比如 `ArrayList` 或 `LinkedList`，然后再根据实现这些接口的集合的特点来选用。

2. 当我们需要保存一组类型相同的数据的时候，我们应该是用一个容器来保存，这个容器就是数组，但是，使用数组存储对象具有一定的弊端。

   ​	**数组的缺点是一旦声明之后，长度就不可变了**；同时，声明数组时的数据类型也决定了该数组存储的数据的类型；而且，数组存储的数据是有序的、可重复的，特点单一。 但是集合提高了数据存储的灵活性，Java 集合不仅可以用来存储不同类型不同数量的对象，还可以保存具有映射关系的数据。



# 二.Collection 子接口之 List



## Arraylist 和 Vector 的区别?

- `ArrayList` 是 `List` 的主要实现类，底层使用动态 `Object[ ]`存储，**适用于频繁的查找工作，线程不安全 ；**
- `Vector` 是 `List` 的古老实现类，底层使用`Object[ ]` 存储，**线程安全的**。





## Arraylist 与 LinkedList 区别?

<img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220507112232230.png" alt="image-20220507112232230" style="zoom: 67%;" />

1. **是否保证线程安全：** `ArrayList` 和 `LinkedList` **都是不同步的，也就是不保证线程安全**；

2. **底层数据结构：** `Arraylist` 底层使用的是 **动态 数组**；`LinkedList` 底层使用的是 **双向链表** 

3. 插入和删除是否受元素位置的影响：

   - `ArrayList` 采用数组存储，所以插入和删除元素的时间复杂度受元素位置的影响。 比如：执行`add(E e)`方法的时候， `ArrayList` 会默认在将指定的元素追加到此列表的末尾，这种情况时间复杂度就是 O(1)。但是如果要在指定位置 i 插入和删除元素的话（`add(int index, E element)`）时间复杂度就为 O(n-i)。因为在进行上述操作的时候集合中第 i 和第 i 个元素之后的(n-i)个元素都要执行向后位/向前移一位的操作。
   - `LinkedList` 采用链表存储，所以，如果是在头尾插入或者删除元素不受元素位置的影响（`add(E e)`、`addFirst(E e)`、`addLast(E e)`、`removeFirst()` 、 `removeLast()`），近似 O(1)，如果是要在指定位置 `i` 插入和删除元素的话（`add(int index, E element)`，`remove(Object o)`） 时间复杂度近似为 O(n) ，因为需要先移动到指定位置再插入。

4. **是否支持快速随机访问：** `LinkedList` 不支持高效的随机元素访问，而 `ArrayList` 支持。快速随机访问就是通过元素的序号快速获取元素对象(对应于`get(int index)`方法)。

   

## 说一说 ArrayList 的扩容机制吧

详见这篇文章 : [ArrayList的扩容机制 - dengrongzhang - 博客园 (cnblogs.com)](https://www.cnblogs.com/dengrongzhang/p/9371551.html)

**`ArrayList`扩容的本质就是计算出新的扩容数组的大小后实例化，并将原有数组内容复制到新数组中去。**

**以无参数构造方法创建 `ArrayList` 时，实际上初始化赋值的是一个空数组。当真正对数组进行添加元素操作时，才真正分配容量。即向数组中添加第一个元素时，数组容量扩为 10。** 

​	![image-20220228201053083](C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220228201053083.png)





# 三.Collection 子接口之 Set



## comparable 和 Comparator 的区别

- `Comparable` :`Comparable`可以认为是一个**内比较器**，实现了`Comparable接口`的类有一个特点，就是这些类是**可以和自己比较的**，至于具体和另一个实现了`Comparable接口`的类如何比较，则依赖`compareTo`方法的实现，`compareTo`()方法也被称为**自然比较方法**。如果开发者进入一个`Collection`的对象想要`Collections.sort()`方法帮你自动进行排序的话，那么这个对象必须实现`Comparable接口`。

- `Comparator`:

  ​	`Comparator`可以认为是是一个**外比较器**，个人认为有两种情况可以使用实现`Comparator接口`的方式：

  1. 一个对象不支持自己和自己比较（没有实现`Comparable接口`），但是又想对两个对象进行比较

  2. 一个对象实现了`Comparable接口`，但是开发者想重写`compareTo()`方法



## 比较 HashSet、LinkedHashSet 和 TreeSet 三者的异同

- `HashSet`、`LinkedHashSet` 和 `TreeSet` 都是 `Set` 接口的实现类，**都是不可重复的，并且都不是线程安全的**。
- `HashSet`、`LinkedHashSet` 和 `TreeSet` 的主要区别在于底层数据结构不同。`HashSet` 的底层数据结构是哈希表（基于 `HashMap` 实现）。`LinkedHashSet` 的底层数据结构是链表和哈希表，元素的插入和取出顺序满足 FIFO。`TreeSet` 底层数据结构是红黑树，元素是有序的，排序的方式有自然排序和定制排序。
- 底层数据结构不同又导致这三者的应用场景不同。`HashSet` 用于不需要保证元素插入和取出顺序的场景，`LinkedHashSet` 用于保证元素的插入和取出顺序满足 FIFO 的场景，`TreeSet` 用于支持对元素自定义排序规则的场景。



# 四.Collection 子接口之 Queue



## Queue 与 Deque 的区别

1. `Queue` 是单端队列，只能从一端插入元素，另一端删除元素，实现上一般遵循 **先进先出（FIFO）** 规则。
2. `Deque` 是双端队列，在队列的两端均可以插入或删除元素。可用于模拟栈



## ArrayDeque 与 LinkedList 的区别

`ArrayDeque` 和 `LinkedList` 都实现了 `Deque` 接口，两者都具有队列的功能，但两者有什么区别呢？

- `ArrayDeque` 是基于可变长的数组和双指针来实现，而 `LinkedList` 则通过链表来实现。
- `ArrayDeque` 不支持存储 `NULL` 数据，但 `LinkedList` 支持。
- `ArrayDeque` 插入时可能存在扩容过程, 不过均摊后的插入操作依然为 O(1)。虽然 `LinkedList` 不需要扩容，但是每次插入数据时均需要申请新的堆空间，均摊性能相比更慢。

从性能的角度上，选用 `ArrayDeque` 来实现队列要比 `LinkedList` 更好。此外，`ArrayDeque` 也可以用于实现栈。



## 说一说 PriorityQueue优先队列

`PriorityQueue` 与 `Queue` 的区别在于元素出队顺序是与优先级相关的，即**总是优先级最高的元素先出队**。

- `PriorityQueue` 利用了二叉堆的数据结构来实现的，底层使用可变长的数组来存储数据







# 五.Map接口



## `HashMap `与` Hashtable` 的区别

<img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220507112421293.png" alt="image-20220507112421293" style="zoom:67%;" />

1. `HashMap`:	**非线程安全**，效率高，可存储`null`的`key`和`value`,但**只可存一个`null`的`key`**

   ​		初始容量&扩充容量大小：

   ① 创建时如果不指定容量初始值，`HashMap` 默认的初始化大小为 16。之后每次扩充，容量变为原来的 2 倍。

   ② 创建时如果给定了容量初始值， `HashMap` 会将其扩充为 `2^n`大小（`HashMap` 中的`tableSizeFor()`方法保证，下面给出了源代码）。也就是说 **`HashMap` 总是使用 2 的幂作为哈希表的大小**。

2. `Hashtable`:  **线程安全**，效率低，**不可存储`null`的`key`和`value`,**使用 `synchronized` 来保证线程安全，使用同一把锁。

   ​		初始容量&扩充容量大小：

   ① 创建时如果不指定容量初始值，`Hashtable` 默认的初始大小为 11，之后每次扩充，容量变为原来的  2倍 + 1。

   ② 创建时如果给定了容量初始值，那么 `Hashtable` 会直接使用你给定的大小。

3. **底层数据结构：**

   1. `JDK1.8` 以后的 `HashMap` 在解决哈希冲突时有了较大的变化，**当链表长度大于阈值（默认为 8）（将链表转换成红黑树前会判断，如果当前数组的长度小于 64，那么会选择先进行数组扩容，而不是转换为红黑树）时，将链表转化为红黑树，以减少搜索时间。**
   2. `Hashtable` 没有这样的机制。





## `HashMap` 和` HashSet` 区别

​	`HashSet` 底层就是基于 `HashMap` 实现的。

|               `HashMap`                |                          `HashSet`                           |
| :------------------------------------: | :----------------------------------------------------------: |
|           实现了 `Map` 接口            |                       实现 `Set` 接口                        |
|               存储键值对               |                        **仅存储对象**                        |
|     调用 `put()`向 map 中添加元素      |             调用 `add()`方法向 `Set` 中添加元素              |
| `HashMap` 使用键（Key）计算 `hashcode` | `HashSet` 使用成员对象来计算 `hashcode` 值，对于两个对象来说 `hashcode` 可能相同，所以`equals()`方法用来判断对象的相等性 |



## `HashMap` 和 `TreeMap `区别

```txt
TreeMap 和 HashMap 都继承自AbstractMap
```

![img](https://snailclimb.gitee.io/javaguide/docs/java/collection/images/TreeMap%E7%BB%A7%E6%89%BF%E7%BB%93%E6%9E%84.png)

相比于`HashMap`来说 ，`TreeMap` 主要多了对 集合中的元素根据键排序的能力 以及 对集合内元素的搜索的能力。因为`TreeMap`它还实现了`SortedMap`和 `NavigableMap`接口接口。



## `HashSet`如何检查重复

当你把对象加入`HashSet`时，`HashSet` 会先计算对象的`hashcode`值来判断对象加入的位置，同时也会与其他加入的对象的 `hashcode` 值作比较，如果没有相符的 `hashcode`，`HashSet` 会假设对象没有重复出现。但是如果发现有相同 `hashcode` 值的对象，这时会调用`equals()`方法来检查 `hashcode` 相等的对象是否真的相同。如果两者相同，`HashSet` 就不会让加入操作成功。

​	**`hashCode()`与 `equals()` 的相关规定：**

1. 如果两个对象相等，则 `hashcode` 一定也是相同的
2. 两个对象相等,对两个 `equals()` 方法返回 true
3. 两个对象有相同的 `hashcode` 值，它们也不一定是相等的
4. 综上，`equals()` 方法被覆盖过，则 `hashCode()` 方法也必须被覆盖
5. `hashCode()`的默认行为是对堆上的对象产生独特值。如果没有重写 `hashCode()`，则该 class 的两个对象无论如何都不会相等（即使这两个对象指向相同的数据）。



## `ConcurrentHashMap`和 `Hashtable` 的区别

`ConcurrentHashMap` 和 `Hashtable` 的区别主要体现在**实现线程安全**的方式上不同。

- **底层数据结构：** `JDK1.7 `的 `ConcurrentHashMap` 底层采用 **分段的数组+链表** 实现，`JDK1.8 `采用的数据结构跟 `HashMap1.8` 的结构一样，数组+链表/红黑二叉树。`Hashtable` 和 `JDK1.8` 之前的 `HashMap` 的底层数据结构类似都是采用 **数组+链表** 的形式，数组是 `HashMap `的主体，链表则是主要为了解决哈希冲突而存在的；
- **实现线程安全的方式（重要）：** 
  - ① `ConcurrentHashMap：`**在 `JDK1.7 `的时候，`ConcurrentHashMap`** 对整个桶数组进行了分割分段,每一把锁只锁容器其中一部分数据，多线程访问容器里不同数据段的数据，就不会存在锁竞争，提高并发访问率。 **到了 `JDK1.8` 的时候直接用 `Node` 数组+链表+红黑树的数据结构来实现，** 整个看起来就像是优化过且线程安全的 `HashMap`。
  - ② **`Hashtable`(同一把锁) :使用 `synchronized` 来保证线程安全，效率非常低下**。当一个线程访问同步方法时，其他线程也访问同步方法，可能会进入阻塞或轮询状态，如使用 put 添加元素，另一个线程不能使用 put 添加元素，也不能使用 get，竞争会越来越激烈效率越低。

**两者的对比图：**

**`Hashtable`:**

![Hashtable全表锁](https://my-blog-to-use.oss-cn-beijing.aliyuncs.com/2019-6/HashTable%E5%85%A8%E8%A1%A8%E9%94%81.png)

**`JDK1.7` 的` ConcurrentHashMap`：**

![JDK1.7的ConcurrentHashMap](https://my-blog-to-use.oss-cn-beijing.aliyuncs.com/2019-6/ConcurrentHashMap%E5%88%86%E6%AE%B5%E9%94%81.jpg)

**`JDK1.8` 的` ConcurrentHashMap`：**

![Java8 ConcurrentHashMap 存储结构（图片来自 javadoop）](https://snailclimb.gitee.io/javaguide/docs/java/collection/images/java8_concurrenthashmap.png)





# 六. 迭代器 Iterator



## 1.  使用迭代器的原因

Collection 定义的方法主要是增加和删除操作，并没有直接查询和修改数据的操作。

- 原因：Collection 细分了三大不同特性的接口，分别代表了三种不同特性的集合。这些集合对数据的处理方式各不相同，如 List 的数据可以通过索引获取数据（**有序且重复**），但 Set 的数据不可以通过索引获取（**无序且不可重复**），所以Collection 不好统一定义直接访问数据的方法。

- Collection 爸爸定义了 **toArray() ：集合 -> 数组** ，这样就可以统一的直接访问数据了。但这样丧失了集合的特性。

- 为了兼顾各子类的特性，同时又能提供**可以访问不同特性集合数据的方法**，Collection 选择了**迭代器模式**。

  - **hasNext() : 用来判断还有没有集合要访问；**

    **Next() : 用来访问集合的下一个数据；**

    <img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220507101454639.png" alt="image-20220507101454639" style="zoom: 67%;" />

  - 集合使用 迭代器是先实现 **Iterable**接口，用该接口定义的方法返回当前集合的迭代器，而 Collection 继承了 Iterable 接口，所以 Collection 体系的集合都要按照这种方式返回迭代器，以供子类接口访问数据。

    > 集合使用 实现 **Iterable**接口 的方式 使用迭代器的原因 ：
    >
    > - 若集合直接实现迭代器的话，别人调用了 当前集合的 Next() 就会影响到你遍历数据。
    >
    > - 而通过实现 **Iterable**接口 使用迭代器，每次都返回新的迭代器，不同迭代器遍历数据时互不影响。也可得出 **迭代器具有独立性 和 隔离性 。**
    >
    > - 并且在 Java 中，**如果实现了 Iterable接口 ，并返回了当前集合的迭代器，那就可以使用 for-each 去遍历数据，既可以省略 hasNext() 和 Next() 。**
    >
    >   <img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220507102711851.png" alt="image-20220507102711851" style="zoom:67%;" />



<img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220507102101810.png" alt="image-20220507102101810" style="zoom:50%;" />

![image-20220507102117144](C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220507102117144.png)

<img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220507102144304.png" alt="image-20220507102144304" style="zoom: 80%;" />

- 总结 ：集合不直接实现 Iterator 接口，而是 实现 Iterable 接口，是为了保证 **迭代器的 独立性 和 隔离性**  且 可以直接使用 for-each 遍历数据。

  > - 独立性：不同迭代器遍历元素时互不影响 。
  > - 隔离性：如果集合 增加 或 删除 元素，不会影响到已有的迭代器。

  

## 2. 迭代器的 快速失败 机制

但 集合不直接实现 Iterator 接口，而是 实现 Iterable 接口 只是 保证 **迭代器的 独立性 和 隔离性**  的前提，Java 标准库中常用集合类的做法是在获取迭代器时让迭代器保存一个 int 数值 **modCount** ，**用来 记录集合增删操作的次数**，当集合增删元素时 ，modCount++ 。

> <img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220507104418420.png" alt="image-20220507104418420" style="zoom:50%;" />

因为迭代器是集合的成员内部类，所以可以随时访问集合的成员属性，**迭代器在遍历时，会检查 modCount 与 当初保存的数值是否一致，若不一致 则表示集合在获取了迭代器后进行了增删操作，迭代器会抛出异常停止迭代。**

> 如 先获取一个当前集合的迭代器，再对集合 新 增删元素，然后再用迭代器访问元素，则迭代器会抛出异常。
>
> <img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220507105046999.png" alt="image-20220507105046999" style="zoom: 33%;" />

- 总结 ：迭代器的 fast-fail 机制 保证了 迭代器的 独立性 和 隔离性，但可能会引发异常。



## 3. 写入时复制 Copy-on-write

Java标准库提供了 CopyOnWriteArrayList 类 实现了在迭代元素的同时，集合还能安全的进行 增删操作，并保证了较高的性能。

- COW 增删元素慢，获取元素快，适用于 读多写少，数据量不大的情况。

- COW 只会复制数据的引用，所以在获取迭代器时速度很快。

<img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220507110952860.png" alt="image-20220507110952860" style="zoom:50%;" />

- 为了 增删元素时 不影响到迭代器，COW 新建一个数组 进行增删操作。

<img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220507110902377.png" alt="image-20220507110902377" style="zoom: 33%;" />
