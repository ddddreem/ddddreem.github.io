---
title: Java集合
date: 2022-12-09 16:51:11
tags: 
- java
- Collection&Map
---

# Java集合



## Collection 接口

Collection 是单列集合，即每次添加的元素时单个的元素，Collection接口继承了 <font color=green size=4>**Iterable接口**</font> ，实现了Iterable的集合可以使用迭代器来遍历集合

基本用法如下：

```java
List list = new ArrayList();
list.add("01");
list.add("02");
list.add("04");
list.add("03");
list.add("05");

while(list.hasNext()){
    Object object = list.next();
    system.out.println(object);
}

// 补充：加强 for, for(元素类型 暂存遍历元素的变量 ：遍历的集合)
for(Object o : list){
    system.out.printf(o);
}
```

<font color=green size=4>**Collection注意事项：**</font>

+ Collection 的实现子类都可以存放多个元素，每个元素可以是 Object
+ 有些Collection 的实现类，可以存放重复的元素，有些不可以
+ 有些Collection 的实现类，有些是有序的（List），有些不是有序的（Set）
+ Collection 接口没有直接的实现子类，是通过她的子接口 Set 和 List 来实现的

![](./img/image-20221208091753534.png)

<font color=green size=4>**Collection 的接口方法：**</font>

Collection接口的实现类实现了Collection的方法，Collection定义了实现Collection接口集合基本的方法

> + add(Object o) : 添加单个元素
> + remove(Object o) : 删除指定下标的元素
> + contains(Object o) : 查询元素是否存在集合中
> + size() : 获取集合的长度，即集合中元素个数
> + isEmpty() : 判断集合是否为空
> + clear() : 清空集合中的所有元素
> + addAll(Collection c) : 添加多个元素
> + containsAll(Collection c) : 查询多个元素是否都存在
> + removeAll(Collection c) : 删除多个元素
> + iterator() : 获取集合的迭代器，用于遍历集合

<!-- more --> 

+++

### List集合及其实现子类

List 集合是 Collection 接口的子接口

<font color=green size=4>**List注意事项：**</font>

+ List 集合类中元素有序（即添加顺序和取出顺序一直），且可以重复
+ List 集合中的每个元素都有其对应的顺序索引，即支持索引操作
+ List 容器中的元素都对应一个整数型的序号记载在起容器的位置，可以使用序号来存取容器中的元素

<font color=green size=4>**List 常用实现类：**</font>**ArrayList、LinkedList 和 Vector**

**ArrayList ：**

![](./img/image-20221208094349977.png)

**LinkedList :** 

![image-20221208094906624](./img/image-20221208094906624.png)

**Vector :** 

![image-20221208094832394](./img/image-20221208094832394.png)

<font color=green size=4>**List 接口方法：**</font>

**由于List 集合为顺序集合，且需要其实现子类底层维护一个 Object 类型的数组或者链表（可以使用下标访问到），所以List 集合根据数组特性添加了根据索引来操作集合元素的方法**

> + void add(int index, Object ele) : 在 index 位置处插入元素 ele
> + boolean addAll(int index, Collection eles) : 从 index 位置开始将 eles中的所有元素都添加进来
> + Object get(int index) : 获取指定 index 位置的元素
> + int indexOf(Object obj) : 返回 obj 在集合中首次出现的位置
> + int lastIndexOf(Object obj) : 返回 obj 在当前集合中最后一次出现的位置
> + Object set(int index, Object ele) : 设置指定 index 位置的元素为 ele，相当于是替换
> + List subList(int formIndex, int toIndex) : 返回从 fromIndex到 toIndex位置的子集合，注意截取的子集合为下表的左闭右开，即 **[fromIndex, toIndex)** 

+++

#### <font color=red size=5>ArrayList</font>



##### <font color=green size=4>ArrayList 注意事项：</font>

+ ArrayList 可以加入 null 值，并且可以是多个
+ ArrayList 是由数组来实现数据存储的，底层维护一个 Object 数组，即 Object[] elementData;
+ ArrayList 基本等同于 Vector，但是由于 ArrayList 没有增加进程同步机制，所以执行效率高；Vector 有进程同步机制，所以多进程下数据是安全的（即 synchronized）

![image-20221208094349977](./img/image-20221208094349977.png)



##### <font color=green size=4>ArrayList 扩容机制：</font>

+ **ArrayList 中维护了一个 Object 类型的数组 elementData；（transient Object[] elementData）其中 transient 表示不能被序列化，即不可以使用网络数据流传输**
+ **当创建集合对象时，如果使用的是无参构造器（ ArrayList() ），则初始 elementData 容量为 0 ；（jdk 是 10）**
+ **当添加元素时：先判断是否需要扩容，如果需要扩容，则调用 grow 方法，否则直接添加元素到合适位置**
+ **如果使用的是无参构造器创建集合对象（ ArrayList() ），如果是第一次添加，elementData 大小扩容到 10，如果需要再次扩容，则在其基础上将elementData数组的长度扩大到原来的 1.5倍；（0 --> 10 --> 15 --> 22 --> 33）**
+ **如果使用的是指定容量 capacity 的构造器（ ArrayList(int initialCapacity) ），则初始 elementData 的大小为给定的 capacity大小；**
+ **如果使用的是指定容量 capacity 的构造器（ ArrayList(int initialCapacity) ），则第一次扩容时，elementData 的大小扩容为指定容量 capacity 的1.5倍；（例如：ArrayList(8)，则她的容量变化为 8 --> 12 --> 18 --> 27）**

> <font color=blue size=4>**tips_01：**</font>发现一个小问题，**当你 new 一个ArrayList对象的时候，他的 size 属性没有显性赋值**。**调用完构造函数之后，size就自动变为 0；而当在 main 方法中声明一个 int 类型的变量而不给初值，输出却编译错误。**
>
> ![image-20221209112409802](./img/image-20221209112409802.png)
>
> ![image-20221209112656262](./img/image-20221209112656262.png)
>
> <font color=blue size=4>**这个问题的原因是：**在新建类的时候，类对象的属性会赋初值，即当一个 int 类型以类的属性的形式存在，则在新建这个类的时候会自动赋初值为 0，这就是为什么 new 一个 ArrayList 对象时，size没有显性赋值，却有初值 0 的原因。</font>
>
> <font color=blue size=4>**tips_02：**</font>默认状态下，idea工具不会显示值为 null 的集合元素，即扩容后的集合，输出看不到 null 的数据。
>
> ![image-20221209124817404](./img/image-20221209124817404.png)

##### <font color=green size=4>无参构造和有参构造的区别：</font>

​	**无参构造和有参构造的主要区别就是初始容量的区别**

+ **无参构造：**调用无参构造方法 **`ArrayList()`**，**`size`** 表示下标的位置，初始化后自动赋值为 **`0`**，维护的初始数组为一个空数组；

  ![](./img/image-20221209120053779.png)

  ![image-20221209120147173](./img/image-20221209120147173.png)

+ **有参构造：**调用有参构造方法 **`ArrayList(int initialCapacity)`**，和无参构造一样，size 也是自动赋值的，底层**`elementData`**数组指向指定 **`initialCapacity`** 大小的新数组，并且指定容量的数组的所有元素都为 `null`；特别的，当指定容量值为 **`0`**时，效果同无参构造一样。

  ![image-20221209131259241](./img/image-20221209131259241.png)
  
  

##### <font color=green size=4>扩容步骤（无参构造）：</font>

**<font color=green size=3>使用无参构造方法构造 ArrayList对象后，第一次添加元素时：</font>**

1. 由于无参构造`new ArrayList()` 初始化的底层数组为空数组，即 **`{}`**；因此第一次添加的时候会自动扩容到 **`10`**；

   ![](./img/image-20221209120053779.png)

2. 添加的时候先调用`ensureCapacityInternal(int)`方法，用来判断此时新增一个元素后的数组长度和 ArrayList对象底层维护的数组的长度的大小，随后调用`ensureExplicitCapacity(int)` 来判断是否需要扩容；
   

   ![image-20221209125138245](./img/image-20221209125138245.png)
   
   ![image-20221209125356879](./img/image-20221209125356879.png)
   
2. 判断当前是否是无参构造方法构造的集合的第一次元素添加，如果是第一次添加，则自动将容量扩容到 `DEFAULT_CAPACITY`，即扩容到 10；
   
   ![image-20221209125717810](./img/image-20221209125717810.png)
   
3. `ensureExplicitCapacity(int)`方法用来判断是否需要扩容，当添加当前元素后的数组大小大于原来底层数组`elementData`的长度的时候，调用`grow(int)`方法来扩容原来的底层数组；这里的 `modCount` 是用来记录集合被修改的次数；

   ![image-20221209130228696](./img/image-20221209130228696.png)

5. `grow(int)`方法是扩容的底层方法，这里`oldCapacity + (oldCapacity >> 1)`可以看出每次扩容变为原来数组长度的 1.5倍；`if(newCapacity - minCapacity < 0){}`这个判断语句针对的是使用无参构造方法的 ArrayList对象第一次添加元素时数组容量自动扩容到 10 的情况；

   **Analysis：**当ArrayList对象使用的是`new ArrayList()`且是第一次添加元素的时候，通过`calculateCapacity(int)`方法自动将数组大小设为 10，所以这里的`oldCapacity`初始是 0，且运行扩容语句后`newCapacity`的值也是 0，所以这里要做这个判断，将数组容量扩容到 10。

   ![image-20221209130747166](./img/image-20221209130747166.png)

5. 完成添加

   ![image-20221209130955086](./img/image-20221209130955086.png)

<font color=green size=3>**使用无参构造方法构造的ArrayList对象在第一次扩容后再发生容量不够的情况：**</font>

1. 第一步还是先判断当前**`elementData`**中是否有足够容量可以新增一个元素；

![image-20221209160317527](./img/image-20221209160317527.png)

2. 先调用**`calculateCapacity(Object[], int)`**方法来计算所需容量大小；

   ![image-20221209160531487](./img/image-20221209160531487.png)

   ![image-20221209160705036](./img/image-20221209160705036.png)

3. 根据**`calculateCapacity(Object[], int)`**方法计算出来的 **`minCapacity`**值来确定是否要扩容；（`minCapacity`表示此次添加的新元素需要**`elementData`**数组的最小大小，**当此时的`elementData`数组的大小 小于 所需最小的大小时，调用扩容方法`grow(int minCapacity)`**）

   ![image-20221209161135402](./img/image-20221209161135402.png)

4. **`grow(int)`**方法，根据`ensureExplicitCapacity(int)`方法传来的 `minCapacity`来判断是否是无参 ArrayList对象的第一次扩容，如果是，则`if(newCapacity - minCapacity < 0)`里的`newCapacity = minCapacity`才会执行；否则直接将新数组容量直接变为原先数组容量的 **`1.5`**倍（由于此时已经进入到 `grow(int)` 方法了，所以此时的 `elementData` 数组的大小已经不能再添加元素了，因此在原先数组的长度基础上变为原来的 **`1.5`**倍）

   ![image-20221209161604486](./img/image-20221209161604486.png)

5. 最后执行 `elementData[size++] = e` 将元素放进数组中。

   ![image-20221209162602339](./img/image-20221209162602339.png)

##### <font color=green size=4>扩容步骤（有参构造）：</font>

+ 在调用有参构造 `ArrayList(int initialCapacity)` 后，当容量达到规定大小的时候，则开始使用扩容机制，将容量扩容到当前`elementData`数组大小的 **`1.5`**倍，其具体操作与无参构造完成第一次扩容后再次容量不足是的操作是一样的。

+++

#### <font color=red size=5>Vector</font>



##### <font color=green size=4>Vector 注意事项：</font>

+ Vector 底层也维护一个对象数组，`protected Object[] elementData;`；
+ Vector 是线程同步的，方法中有 `synchronized` 字段，即保证线程安全；
+ 相对于 `Arraylist` ，`Vector` 线程安全，执行效率低；`ArrayList` 执行效率高，线程不安全。



##### <font color=green size=4>Vector 扩容机制：</font>

+++

#### <font color=red size=5>LinkedList</font>



##### <font color=green size=4>LinkedList 注意事项：</font>

### Set集合及其实现子类



## Map 接口