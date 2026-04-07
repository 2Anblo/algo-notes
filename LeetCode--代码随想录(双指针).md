# 27.移除元素

## 题目描述

给你一个数组 `nums` 和一个值 `val`，你需要 **[原地](https://baike.baidu.com/item/原地算法)** 移除所有数值等于 `val` 的元素。元素的顺序可能发生改变。然后返回 `nums` 中与 `val` 不同的元素的数量。

假设 `nums` 中不等于 `val` 的元素数量为 `k`，要通过此题，您需要执行以下操作：

- 更改 `nums` 数组，使 `nums` 的前 `k` 个元素包含不等于 `val` 的元素。`nums` 的其余元素和 `nums` 的大小并不重要。
- 返回 `k`。

**用户评测：**

评测机将使用以下代码测试您的解决方案：

```
int[] nums = [...]; // 输入数组
int val = ...; // 要移除的值
int[] expectedNums = [...]; // 长度正确的预期答案。
                            // 它以不等于 val 的值排序。

int k = removeElement(nums, val); // 调用你的实现

assert k == expectedNums.length;
sort(nums, 0, k); // 排序 nums 的前 k 个元素
for (int i = 0; i < actualLength; i++) {
    assert nums[i] == expectedNums[i];
}
```

如果所有的断言都通过，你的解决方案将会 **通过**。

 

**示例 1：**

```
输入：nums = [3,2,2,3], val = 3
输出：2, nums = [2,2,_,_]
解释：你的函数应该返回 k = 2, 并且 nums 中的前两个元素均为 2。
你在返回的 k 个元素之外留下了什么并不重要（因此它们并不计入评测）。
```

**示例 2：**

```
输入：nums = [0,1,2,2,3,0,4,2], val = 2
输出：5, nums = [0,1,4,0,3,_,_,_]
解释：你的函数应该返回 k = 5，并且 nums 中的前五个元素为 0,0,1,3,4。
注意这五个元素可以任意顺序返回。
你在返回的 k 个元素之外留下了什么并不重要（因此它们并不计入评测）。
```

 

**提示：**

- `0 <= nums.length <= 100`
- `0 <= nums[i] <= 50`
- `0 <= val <= 100`

##  解题思路

双指针法（快慢指针法）： **通过一个快指针和慢指针在一个for循环下完成两个for循环的工作。**

定义快慢指针

- 快指针：寻找新数组的元素 ，新数组就是不含有目标元素的数组
- 慢指针：指向更新 新数组下标的位置

这里我们可以把快指针理解为`读指针（read）`，慢指针理解为`写指针（write）`；只有当`read`指针的值不与要删除的值`val`相等时，我们才更新`write`写指针，并移动写指针。

这样一来，写指针更新的次数，即为`新数组的大小`。

![image-20260321200548862](./LeetCode--代码随想录(双指针).assets/image-20260321200548862.png)

![image-20260321200610965](./LeetCode--代码随想录(双指针).assets/image-20260321200610965.png)

## 代码

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int write = 0;
        // 用read指针遍历数组nums
        for(int read = 0; read < nums.length; read++){
            // 只有当读指针所指向的值不等于val时，更新write指针
            if(val != nums[read]){
                nums[write++] = nums[read];
            }
        }
        // 写指针更新的次数，即为新数组的大小
        return write;
    }
}
```

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

![image-20260407200520652](./LeetCode--代码随想录(双指针).assets/image-20260407200520652.png)

举例：

![image-20260407200603944](./LeetCode--代码随想录(双指针).assets/image-20260407200603944.png)

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

![image-20260407210452417](./LeetCode--代码随想录(双指针).assets/image-20260407210452417.png)

![image-20260407210456247](./LeetCode--代码随想录(双指针).assets/image-20260407210456247.png)

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

