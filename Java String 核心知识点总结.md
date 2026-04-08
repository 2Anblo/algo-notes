# 1、String类的常用方法

## 1.1 获取字符串信息

`int length()`

返回字符串的长度  
```java
int len = str.length();
```

`char charAt(int index)`

返回指定索引处的字符

```java
char c = str.charAt(0);
```

`boolean isEmpty()`

判断字符串是否为空（长度是否为 0）

```java
boolean empty = str.isEmpty();
```

------

## 1.2 大小写转换

`String toLowerCase()`

将字符串转换为小写（默认语言环境）

```java
String lower = str.toLowerCase();
```

`String toUpperCase()`

将字符串转换为大写（默认语言环境）

```java
String upper = str.toUpperCase();
```

------

## 1.3 字符串处理

`String trim()`

去除字符串首尾的空白字符

```java
String result = str.trim();
```

`String substring(int beginIndex)`

从指定位置开始截取子字符串

```java
String sub = str.substring(2);
```

`String substring(int beginIndex, int endIndex)`

截取指定范围的子字符串（左闭右开）

```java
String sub = str.substring(2, 5);
```

------

## 1.4 字符串比较

`boolean equals(Object obj)`

比较字符串内容是否相同

```java
boolean flag = str1.equals(str2);
```

`boolean equalsIgnoreCase(String anotherString)`

比较字符串内容是否相同（忽略大小写）

```java
boolean flag = str1.equalsIgnoreCase(str2);
```

`int compareTo(String anotherString)`

比较两个字符串的大小（按字典顺序）

```java
int result = str1.compareTo(str2);
```

------

## 1.5 字符串拼接

`String concat(String str)`

将指定字符串拼接到当前字符串末尾（等价于 `+`）

```java
String result = str1.concat(str2);
// 或
String result = str1 + str2;
```

----

## 1.6 字符串查找与匹配 

`boolean endsWith(String suffix)`

测试此字符串是否以指定的后缀结束

```java
String str1 = "helloworld";
boolean b1 = str1.endsWith("rld");
```

`boolean startsWith(String prefix)`

测试此字符串是否以指定的前缀开始

```java
String str1 = "helloworld";
boolean b2 = str1.startsWith("He");
```

`boolean startswith(String prefix，int toffset)`

测试此字符串从指定索引开始的子字符串是否以指定的前缀开始

```java
String str1 = "helloworld";
boolean b3 = str1.startsWith("11", 2);
```

`boolean contains(CharSequence s)`

当且仅当此字符串包含指定的 char 值序列时，返回true

```java
String str1 = "helloworld";
String str2 = "wor";
System.out.println(str1.contains(str2));
```

`int indexOf(String str)`

返回指定子字符串在此字符串中第一次出现处的索引

```java
String str1 = "helloworld";
System.out.println(str1.indexOf("lol"));
```

`int indexOf(String str,int fromIndex)`

返回指定子字符串在此字符串中第一次出现处的索引，从指定的索引开始

```java
String str1 = "helloworld";
System.out.println(str1.indexOf("lo",5));
```

`int lastIndexOf(String str)`

返回指定子字符串在此字符串中最右边出现处的索引

```java
String str3 = "hellorworld";
System.out.println(str3.lastIndexOf("or"));
```

`int lastlndexOf(String str,int fromlndex)`

返回指定子字符串在此字符串中最后次出现处的索引，从指定的索引开始**反向搜索**（从后往前找）

```java
String str3 = "hellorworld";
System.out.println(str3.lastIndexOf("or", 6));
```

注：`indexOf`和`lastlndexOf`方法如果未找到都是返回`-1`

---

## 1.7 字符串替换方法

`String replace(char oldChar,char newChar)`

返回一个新的字符串，它是通过用newChar替换此字符串中出现的所有oldchar得到的。

````java
String str1 = "咕噜咕噜咚";
String str2 = str1.replace('咕','嘟');
System.out.println(str1);
System.out.println(str2);

````

`String replace(CharSequence target, CharSequence replacement)`

使用指定的字面值替换序列替换此字符串所有匹配字面值目标序列的子字符串。

```java
String str1 = "咕噜咕噜咚";
String str3 = str1.replace("咕噜", "嘟呜");
System.out.println(str3);
```

---

## 1.8 正则匹配与替换

`String replaceAll(String regex, String replacement)`

使用给定的replacement替换此字符串所有匹配给定的正则表达式的子字符串。

```java
String str = "123java456mysql21345spring215mybatis4444"
// 把字符串中的数字替换成，，如果结果中开头和结尾有，的话去掉
String string = str.replaceAll("\\d+",",").replaceAll("^,|,$","");
System.out.println(string);
```

`String replaceFirst(String regex, String replacement)`

使用给定的replacement替换此字符串匹配给定的正则表达式的第一个子字符串。

```java
String str = "12345";//判断str字符串中是否全部有数字组成，即有1-n个数字组成
boolean matches = str.matches("\\d+");
System.out.println(matches);
```

`boolean matches(String regex)`

告知此字符串是否匹配给定的正则表达式。

```java
String tel = "0571-4534289";
//判断这是否是一个杭州的固定电话
boolean result = tel.matches("0571-\\d{7,8}");
System.out.println(result);
```

---

## 1.9 字符串拆分

`String[] split(String regex)`

根据给定正则表达式的匹配拆分此字符串。

```java
String str = "hello|world|java";
String[] strs = str.split("\\|");
for (int i = 0; i < strs.length; i++) {
    System.out.println(strs[i]);
}
```

`String[] split(String regex,int limit)`

根据匹配给定的正则表达式来拆分此字符串，最多不超过limit个，如果超过了，剩下的全部都放到最后一个元素中。

```java
String str2 = "hello.world.java";
String[] strs2 = str2.split("\\.");
for (int i = 0; i < strs2.length; i++){
    System.out.println(strs2[i]);
}
```

# 2、String与基本数据类型转换

- 字符串→基本数据类型、包装类
  - `Integer包装类`的`public static int parselnt(String s)`方法可以将由 “**数字**”字符组成的字符串转换为整型。
  - 类似地，使用java.lang包中的Byte、Short、Long、Float、Double类调相应的类方法可以将由“**数字**”字符组成的字符串，转化为相应的基本数据类型。
- 基本数据类型、包装类→字符串
  - 调用`String类`的`public String valueOf(int n)`可将`int型`转换为字符串
  - 相应`valueOf(byte b)、valueOf(long I)、valueOf(float f)、valueOf(doubled)、valueOf(boolean b)`可由参数的相应类型到字符串的转换

```java
String str1 = "123";
// 字符串 -> int
// int num = (int) str1; //错误的
int num = Integer.parseInt(str1);
// int -> 字符串
String str2 = String.valueOf(num);
String str3 = num+"";
System.out.println(str2);
System.out.println(str3);
```



