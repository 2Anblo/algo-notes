# Java 集合体系详解：Collection、List、Set、Map

## 1、数组与集合

### 1.1、集合与数组存储数据概述

集合、数组都是对多个数据进行存储操作的结构，简称Java容器

说明：此时的存储，主要指的是内存层面的存储，不涉及到持久化的存储(.txt，.jpg，.avi，数据库中)

### 1.2、数组存储的特点

一旦初始化以后，其长度就确定了。

数组一旦定义好，其元素的类型也就确定了。我们也就只能操作指定类型的数据了。

比如:String[] arr;int[] arr1;Object[] arr2;

### 1.3、数组存储的弊端

一旦初始化以后，其长度就不可修改。

数组中提供的方法非常限，对于添加、删除、插入数据等操作，非常不便，同时效率不高。

获取数组中实际元素的个数的需求，数组没有现成的属性或方法可用

数组存储数据的特点：有序、可重复。对于无序、不可重复的需求，不能满足。

### 1.4、集合存储的优点

解决数组存储数据方面的弊端。

## 2、Collection接口

### 2.1、单列集合框架结构

Collection接口：单列集合，用来存储一个一个的对象

List接口：存储序的、可重复的数据。 →“动态”数组，替换原有的数组

面试题：ArrayList、LinkedList、Vector三者的异同？

同：三个类都是实现了List接口，存储数据的特点相同：存储有序的、可重复的数据

不同：

- Arraylist：作为List接口的主要实现类，线程不安全的，效率高；底层使用Object[] elementData存储
- Linkedlist ：对于频繁的插入、删除操作，使用此类效率比ArrayList高；底层使用双向链表存储
- Vector：作为List接口的古老实现类，线程安全的，效率低；底层使用Object[] elementData存储

Set接口:存储无序的、不可重复的数据→高中讲的“集合”

- HashSet、 LinkedHashSet、 TreeSet

对应图示：

![image-20260329165239537](./Java 集合体系详解：Collection、List、Set、Map.assets/image-20260329165239537.png)

### 2.2、ArrayList

ArrayList的源码分析：

**jdk 7情况下**

```java
ArrayList list= new Arraylist();// 底层创建了长度是10的Object[]数组elementData
list.add(123);// elementData[e] = new Integer(123);
...
list.add(11);// 如果此次的添加导致底层elementData数组容量不够，则扩容
// 默认情况下，扩容为原来的容量的1.5倍，同时需要将原有数组中的数据复制到新的数组中。

// 结论:建议开发中使用带参的构造器:ArrayList List = new ArrayList(int capacity)
```

**jdk 8中ArrayList的变化**

```java
ArrayList list= new Arraylist();// 底层Object[]elementData初始化为{}，并没有创建长度为10的数组
list.add(123);// 第一次调用add()时，底层才创建了长度10的数组，并将数据123添加到数组elementData
...
// 后续的添加和扩容操作与jdk7无异。
```

小结：jdk7中的ArrayList的对象的创建类似于单例的饿汉式，而jdk8中的ArrayList的对象的创建类似于单例的懒汉式，延迟了数组的创建，节省内存。

### 2.3、LinkedList

LinkedList的源码分析：

```java
LinkedList list= new LinkedList();// 内部声明了Node类型的first和Last属性，默认值为null
List.add(123);// 将123封装到Node中，创建了Node对象。
// 其中，Node定义为: 体现了LinkedList的双向链表的说法
private static class Node<E> {
    E item;
    Node<E> next;
    Node<E> prev;

    Node(Node<E> prev, E element, Node<E> next) {
        this.item = element;
        this.next = next;
        this.prev = prev;
    }
}
```

### 2.4、List接口中的常用方法

```java
/**
void add(int index,Object ele):在index位置插入ele元素

boolean addAll(int index,Collection eles):从index位置开始将eles中的所有元素添加进来

Object get(int index):获取指定index位置的元素

int indexof(object obj):返回obj在集合中首次出现的位置，如果不存在，返回-1

int LastIndexOf(0bject obj):返回obj在当前集合中末次出现的位置，如果不存在，返回-1

Object remove(int index):移除指定index位置的元素，并返回此元素

Object set(int index，Object ele):设置指定index位置的元素为ele

List sublist(int fromIndex, int toIndex):返回从fromIndex到toIndex位置的左闭右开子集合

*/
```

**总结:常用方法**
增：`add(Object obj)`

删：`remove(int index)/ remove(Object obj)`

改：`set(int index,Object ele)`

查：`get(int index)`

插： `add(int index, Object ele)`

长度：`size()`

遍历：① Iterator迭代器方式

​	   ② 增强for循环

​	   ③ 普通的循环