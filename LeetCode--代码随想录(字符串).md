# 344.反转字符串

## 题目描述

编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 `s` 的形式给出。

不要给另外的数组分配额外的空间，你必须**[原地](https://baike.baidu.com/item/原地算法)修改输入数组**、使用 O(1) 的额外空间解决这一问题。

 

**示例 1：**

```
输入：s = ["h","e","l","l","o"]
输出：["o","l","l","e","h"]
```

**示例 2：**

```
输入：s = ["H","a","n","n","a","h"]
输出：["h","a","n","n","a","H"]
```

 

**提示：**

- `1 <= s.length <= 105`
- `s[i]` 都是 [ASCII](https://baike.baidu.com/item/ASCII) 码表中的可打印字符

## 解题思路

在这里可以使用异或（XOR）来实现不创建临时变量的交换方法。

因为：

1、异或满足交换律

2、异或满足结合律

3、A^A=0

![image-20260407200520652](./LeetCode--代码随想录(字符串).assets/image-20260407200520652.png)

举例：

![image-20260407200603944](./LeetCode--代码随想录(字符串).assets/image-20260407200603944.png)

## 代码

```java
class Solution {
    public void reverseString(char[] s) {
        // 定义左右指针
        int left = 0;
        int right = s.length - 1;

        while(left<=right){
            // 创建临时变量
            char temp;
            temp = s[left];
            s[left++] = s[right];
            s[right--] = temp;
        }
        
    }
}
```

使用异或：

```java
class Solution {
    public void reverseString(char[] s) {
        // 定义左右指针
        int left = 0;
        int right = s.length - 1;

        while(left < right){
            // 使用异或此处不能取等 否则A^A=0
            s[left] ^= s[right];
            s[right] ^= s[left];
            s[left++] ^= s[right--];
        }
        
    }
}
```

# 541.反转字符串 II

## 题目描述

给定一个字符串 `s` 和一个整数 `k`，从字符串开头算起，每计数至 `2k` 个字符，就反转这 `2k` 字符中的前 `k` 个字符。

- 如果剩余字符少于 `k` 个，则将剩余字符全部反转。
- 如果剩余字符小于 `2k` 但大于或等于 `k` 个，则反转前 `k` 个字符，其余字符保持原样。

 

**示例 1：**

```
输入：s = "abcdefg", k = 2
输出："bacdfeg"
```

**示例 2：**

```
输入：s = "abcd", k = 2
输出："bacd"
```

 

**提示：**

- `1 <= s.length <= 104`
- `s` 仅由小写英文组成
- `1 <= k <= 104`

## 代码

```java
class Solution {

    public static void reverse(char[] s, int start, int end){
        // 做成左闭右开区间
        end--;
        while(start < end){
            s[start] ^= s[end];
            s[end] ^= s[start];
            s[start++] ^= s[end--];
        }
    }

    public String reverseStr(String s, int k) {
        // 转换成字符数组进行操作
        char[] chars = s.toCharArray();
        int start = 0;
        // 2k为一组，减少遍历循环次数
        for(int i=0; i<chars.length; i+=2*k){
            reverse(chars, i, Math.min((i+k),chars.length));
        }
        return new String(chars);
    }
}
```

# 54.替换数字

## 题目描述

给定一个字符串 s，它包含小写字母和数字字符，请编写一个函数，将字符串中的字母字符保持不变，而将每个数字字符替换为number。 例如，对于输入字符串 "a1b2c3"，函数应该将其转换为 "anumberbnumbercnumber"。

输入描述

输入一个字符串 s,s 仅包含小写字母和数字字符。

输出描述

打印一个新的字符串，其中每个数字字符都被替换为了number

输入示例

```
a1b2c3
```

输出示例

```
anumberbnumbercnumber
```

提示信息

数据范围：
1 <= s.length < 10000。

## 解题思路

利用双指针：

![image-20260407210452417](./LeetCode--代码随想录(字符串).assets/image-20260407210452417.png)

![image-20260407210456247](./LeetCode--代码随想录(字符串).assets/image-20260407210456247.png)

## 代码

解法一（StringBuilder）：

```java
import java.util.*;

public class Main{

    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        // 因为java中的String并不能扩充，借助StringBuilder完成字符串操作
        StringBuilder sb = new StringBuilder();
        String input = sc.nextLine();
        char[] inputArray = input.toCharArray();

        for(int i=0; i<inputArray.length; i++){
            if(inputArray[i]>='0' && inputArray[i]<='9'){
                // 遇到数字则替换为number
                sb.append("number");
            } else{
                sb.append(inputArray[i]);
            }
        }
        System.out.println(sb.toString());

        sc.close();
    }

}
```

解法二（双指针法）：

```java
import java.util.*;

public class Main{

    public static String replaceNumber(String s){
        // 转换为字符数组
        char[] sArray = s.toCharArray();
        // 为重构字符串输数字个数
        int count = 0;
        for(int i=0; i<sArray.length; i++){
            if(sArray[i]>='0' && sArray[i]<='9'){
                count++;
            }
        }
        // 重构字符串数组
        char[] newArray = new char[sArray.length + count*5];
        // 给定右指针right
        int right = newArray.length - 1;
        for(int left=sArray.length - 1; left>=0; left--){
            if(sArray[left]>='0' && sArray[left]<='9'){
                // 从后到前填充number
                newArray[right--] = 'r';
                newArray[right--] = 'e';
                newArray[right--] = 'b';
                newArray[right--] = 'm';
                newArray[right--] = 'u';
                newArray[right--] = 'n';
            } else{
                // 填充原来字母
                newArray[right--] = sArray[left];
            }
        }
        return new String(newArray);

    }

    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        // 读取输入字符串并打印
        System.out.println(replaceNumber(sc.nextLine()));

        sc.close();
    }

}
```

# 151.反转字符串中的单词

## 题目描述

给你一个字符串 `s` ，请你反转字符串中 **单词** 的顺序。

**单词** 是由非空格字符组成的字符串。`s` 中使用至少一个空格将字符串中的 **单词** 分隔开。

返回 **单词** 顺序颠倒且 **单词** 之间用单个空格连接的结果字符串。

**注意：**输入字符串 `s`中可能会存在前导空格、尾随空格或者单词间的多个空格。返回的结果字符串中，单词间应当仅用单个空格分隔，且不包含任何额外的空格。

 

**示例 1：**

```
输入：s = "the sky is blue"
输出："blue is sky the"
```

**示例 2：**

```
输入：s = "  hello world  "
输出："world hello"
解释：反转后的字符串中不能存在前导空格和尾随空格。
```

**示例 3：**

```
输入：s = "a good   example"
输出："example good a"
解释：如果两个单词间有多余的空格，反转后的字符串需要将单词间的空格减少到仅有一个。
```

 

**提示：**

- `1 <= s.length <= 104`
- `s` 包含英文大小写字母、数字和空格 `' '`
- `s` 中 **至少存在一个** 单词

 

**进阶：**如果字符串在你使用的编程语言中是一种可变数据类型，请尝试使用 `O(1)` 额外空间复杂度的 **原地** 解法。

## 解题思路

1、移除字符串中多余的空格，并重建字符串

![image-20260408103740570](./LeetCode--代码随想录(字符串).assets/image-20260408103740570.png)

2、反转字符串

3、反转字符串中的单词

![image-20260408103758233](./LeetCode--代码随想录(字符串).assets/image-20260408103758233.png)

## 代码

```java
class Solution {

    public static char[] removeSpace(String s){
        // 使用字符数组
        char[] array = s.toCharArray();
        // 用快慢指针移除空格
        // 慢指针用于写数据
        int slow = 0;
        // 快指针用于读数据
        for(int fast=0; fast<array.length; fast++){
            // 如果读到不是空格再写
            if(array[fast] != ' '){
                if(slow != 0){
                    // 为该单词前加上空格
                    array[slow++] = ' ';
                }
                //写完一个单词
                while(fast<array.length && array[fast] != ' '){
                    array[slow++] = array[fast++];
                }
            }
        }

        // resize数组大小为slow大小
        char[] newArray = new char[slow];
        for(int i=0; i<slow; i++){
            newArray[i]=array[i];
        }
        return  newArray;
    }

    public static char[] reverseCharArray(char[] s, int start, int end){
        // 反转字符串数组
        int left = start;
        int right = end - 1;
        while(left < right){
            s[left] ^= s[right];
            s[right] ^= s[left];
            s[left++] ^= s[right--];
        }
        return s;
    }

    public static String reverseWordInCharArray(char[] s){
        // 反转每个单词
        int start = 0;
        for(int i=0; i<=s.length; i++){
            // 反转条件判断
            if(i==s.length || s[i]==' '){
                int end = i;
                s = reverseCharArray(s,start,end);
                start=i+1;
            }
        }
        return new String(s);
    }

    public String reverseWords(String s) {
        // 移除字符串中多余的空格
        char[] sArray = removeSpace(s);
        // 反转字符串数组
        sArray = reverseCharArray(sArray,0,sArray.length);
        // 反转单词
        s = reverseWordInCharArray(sArray);

        return s;
    }
}
```

# 55.右旋字符串（第八期模拟笔试）

## 题目描述

字符串的右旋转操作是把字符串尾部的若干个字符转移到字符串的前面。给定一个字符串 s 和一个正整数 k，请编写一个函数，将字符串中的后面 k 个字符移到字符串的前面，实现字符串的右旋转操作。 

例如，对于输入字符串 "abcdefg" 和整数 2，函数应该将其转换为 "fgabcde"。

输入描述

输入共包含两行，第一行为一个正整数 k，代表右旋转的位数。第二行为字符串 s，代表需要旋转的字符串。

输出描述

输出共一行，为进行了右旋转操作后的字符串。

输入示例

```
2
abcdefg
```

输出示例

```
fgabcde
```

提示信息

数据范围：
1 <= k < 10000,
1 <= s.length < 10000;

## 代码

除字符数组外不额外开辟空间：

```java
import java.util.*;

public class Main{

    public static void reverseString(char[] s, int start, int end){
        // 实现左闭右闭区间反转字符串
        while(start < end){
            s[start] ^= s[end];
            s[end] ^= s[start];
            s[start++] ^= s[end--];
        }
    }

    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int num = sc.nextInt();
        sc.nextLine();
        String s = sc.nextLine();
        // 转换成字符数组
        char[] chars = s.toCharArray();
        // 反转整个字符数组
        reverseString(chars, 0, chars.length-1);
        // 反转前num个字符
        reverseString(chars, 0, num - 1);
        // 反转后面length-num个字符
        reverseString(chars, num, chars.length-1);
        System.out.println(new String(chars));
        sc.close();
    }
}
```

额外开辟空间的BF算法：

```java
import java.util.*;
 
public class Main{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int num = sc.nextInt();
        sc.nextLine();
        String s = sc.nextLine();
        // 转换成字符数组
        char[] chars = s.toCharArray();
        char[] lastChars = new char[num];
        for(int i=chars.length-1; i>chars.length-1-num; i--){
            // 存储最后num个字符
            lastChars[i-chars.length+num] = chars[i];
        }
        // 将所有字符向后移动num位
        for(int i=chars.length-1; i>num-1; i--){
            chars[i] = chars[i-num];
        }
        for(int i=0; i<num; i++){
            chars[i] = lastChars[i];
        }
        System.out.println(new String(chars));
        sc.close();
    }
}
```

