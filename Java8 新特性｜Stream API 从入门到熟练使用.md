# 1、Lambda表达式

## 1.1、为什么使用Lambda表达式

Lambda是一个**匿名函数**，我们可以把Lambda表达式理解为是一段可以**传递的代码**（将代码像数据一样进行传递）。使用它可以写出更简洁、更灵活的代码。作为一种更紧凑的代码风格，使Java的语言表达能力得到了提升。

**Lambda表达式的本质**：作为**函数式接口**的实例

## 1.2、Lambda表达式的使用

### 1.2.1、举例

像重写Comparator中的compare方法：

```java
(o1,o2) -> Integer.compare(o1,o2);
```

### 1.2.2、格式

`->`：lambda操作符 或 箭头操作符

`->左边`：Lambda形参列表 (其实就是接口中的抽象方法的形参列表)

`->右边`：Lambda体(其实就是重写的抽象方法的方法体)

## 1.3、Lambda表达式的六种使用情况

`->左边`：Lambda形参列表的参数类型可以省略(类型推断)；如果Lambda形参列表只有一个参数，则那一对( )也可以省略。

`->右边`：Lambda体应该使用一对{}包裹：如果Lambda体只有一条执行语句(可能是return语句)，可以省略这一对{}和return关键字。

### 1.3.1、语法格式一：无参，无返回值

示例代码：

```java
    @Test
    public void test() {

        Runnable r1 = new Runnable() {
            @Override
            public void run() {
                System.out.println("Hello World!");
            }
        };
        r1.run();

        System.out.println("*******************************");
		// Lambda表达式重写
        Runnable r2 = () -> System.out.println("Bye World!");
        r2.run();

    }
```

### 1.3.2、语法格式二：Lambda 需要一个参数，但是没有返回值

示例代码：

```java
    @Test
    public void test2() {

        Consumer<String> c1 = new Consumer<String>() {
            @Override
            public void accept(String s) {
                System.out.println(s);
            }
        };

        c1.accept("Hello World!");
        System.out.println("*******************************");
        
        // 给定参数 String s
        Consumer<String> c2 = (String s) -> System.out.println(s);
        c2.accept("Bye World!");

    }
```

### 1.3.3、语法格式三：数据类型可以省略，因为可由编译器推断得出，称为“类型推断”

示例代码：

```java
    @Test
    public void test3() {

        // 给定参数 String s
        Consumer<String> c1 = (String s) -> System.out.println(s);
        c1.accept("Hello World!");

        System.out.println("*******************************");

        // 给定参数 s
        // 编译器推断得出参数类型
        Consumer<String> c2 = (s) -> System.out.println(s);
        c2.accept("Bye World!");

    }
```

### 1.3.4、语法格式四：Lambda若只需要一个参数时，参数的小括号可以省略

示例代码：

```java
@Test
public void test4() {
    // 给定参数 String s
    Consumer<String> c1 = (s) -> System.out.println(s);
    c1.accept("Hello World!");

    System.out.println("*******************************");

    // 给定参数 s
    // Lambda若只需要一个参数时，参数的小括号可以省略
    Consumer<String> c2 = s -> System.out.println(s);
    c2.accept("Bye World!");
}
```

### 1.3.5、语法格式五：Lambda 需要两个或以上的参数，多条执行语句，并且可以有返回值

示例代码：

```java
@Test
public void test5() {

    Comparator<Integer> com1 = new Comparator<Integer>() {
        @Override
        public int compare(Integer o1, Integer o2) {
            System.out.println(o1);
            System.out.println(o2);
            return o1.compareTo(o2);
        }
    };

    System.out.println(com1.compare(1, 2));
    System.out.println("*************************************");

    Comparator<Integer> com2 = (o1, o2) -> {
        System.out.println(o1);
        System.out.println(o2);
        return o1.compareTo(o2);
    };
    System.out.println(com2.compare(12, 2));

}
```

### 1.3.6、语法格式六：当Lambda体只有一条语句时，若有return 与大括号，都可以省略

示例代码：

```java
@Test
public void test6() {

    Comparator<Integer> com1 = (o1, o2) -> {return o1.compareTo(o2);};

    System.out.println(com1.compare(1, 2));
    System.out.println("*************************************");

    Comparator<Integer> com2 = (o1, o2) -> o1.compareTo(o2);
    System.out.println(com2.compare(12, 2));

}
```

# 2、Stream API

## 2.1、Stream API说明

Java 8中有两大最为重要的改变。

第一个是**Lambda表达式**；另外一个则是 **Stream API**。

`Stream API (java.util.stream)`把真正的函数式编程风格引入到Java中。

这是目前为止对Java类库最好的补充，因为Stream API可以极大提供Java程序员的生产力，让程序员写出高效率、干净、简洁的代码。

Stream 是 Java8 中处理集合的关键抽象概念，它可以指定你希望对集合进行的操作，可以执行非常复杂的查找、过滤和映射数据等操作。

**使用 Stream API 对集合数据进行操作，就类似于使用 SQL 执行的数据库查询。**也可以使用 Stream API 来并行执行操作。简言之，Stream API提供了一种高效且易于使用的处理数据的方式。

## 2.2、为什么要使用Stream API

实际开发中，项目中多数数据源都来自于Mysql，Oracle等。但现在数据源可以更多了，有MongoDB，Redis等，而这些NoSQL的数据就需要Java层面去处理。

Stream 和 Collection集合的区别：**Collection是一种静态的内存数据结构，而Stream是有关计算的。**前者是主要面向内存，存储在内存中，后者主要是面向CPU，通过 CPU实现计算。

## 2.3、什么是 Stream

**Stream到底是什么呢?**
是数据渠道，用于操作数据源（集合、数组等）所生成的元素序列。

“集合讲的是数据，Stream讲的是计算!”

**注意：**
1、Stream自己不会存储元素。
2、Stream不会改变源对象。相反，他们会返回一个持有结果的新Stream。
3、Stream操作是延迟执行的。这意味着他们会等到需要结果的时候才执行。

## 2.4、Stream 操作的三个步骤

1、创建 Stream

一个数据源（如：集合、数组），获取一个流。

2、中间操作
一个中间操作链，对数据源的数据进行处理

3、终止操作（终端操作）
一旦执行终止操作，就执行中间操作链，并产生结果。之后，不会再被使用

![image-20260405193346521](./Java8 新特性｜Stream API 从入门到熟练使用.assets/image-20260405193346521.png)