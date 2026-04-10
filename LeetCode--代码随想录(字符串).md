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

