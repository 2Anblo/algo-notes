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

![image-20260408103740570](./LeetCode--代码随想录(双指针).assets/image-20260408103740570.png)

2、反转字符串

3、反转字符串中的单词

![image-20260408103758233](./LeetCode--代码随想录(双指针).assets/image-20260408103758233.png)

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

# 206.反转链表

## 题目描述

给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表。

 

**示例 1：**

![img](./LeetCode--代码随想录(双指针).assets/rev1ex1.jpg)

```
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
```

**示例 2：**

![img](./LeetCode--代码随想录(双指针).assets/rev1ex2.jpg)

```
输入：head = [1,2]
输出：[2,1]
```

**示例 3：**

```
输入：head = []
输出：[]
```

 

**提示：**

- 链表中节点的数目范围是 `[0, 5000]`
- `-5000 <= Node.val <= 5000`

 

**进阶：**链表可以选用迭代或递归方式完成反转。你能否用两种方法解决这道题？

## 解题思路

### 双指针法

![image-20260325203946407](./LeetCode--代码随想录(双指针).assets/image-20260325203946407.png)

![image-20260325203959618](./LeetCode--代码随想录(双指针).assets/image-20260325203959618.png)



## 代码

### 双指针法

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        // 从头结点开始遍历
        ListNode current = head;
        // 记录先前结点
        ListNode prev = head;
        while(current != null){
            ListNode next = current.next;
            // 边缘条件判断
            if(current == head){
                current.next = null;
                current = next;
            } else{
                current.next = prev;
                prev = current;
                current = next;
            }
        }

        return prev;
    }
}
```

# 19.删除链表的倒数第 N 个结点

## 题目描述

给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。

 

**示例 1：**

![img](./LeetCode--代码随想录(双指针).assets/remove_ex1.jpg)

```
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
```

**示例 2：**

```
输入：head = [1], n = 1
输出：[]
```

**示例 3：**

```
输入：head = [1,2], n = 1
输出：[1]
```

 

**提示：**

- 链表中结点的数目为 `sz`
- `1 <= sz <= 30`
- `0 <= Node.val <= 100`
- `1 <= n <= sz`

## 解题思路

使用快慢指针和虚拟头结点来实现一趟扫描删除倒数第N个结点。

以删除倒数第2个结点为例：

![image-20260326214448118](./LeetCode--代码随想录(双指针).assets/image-20260326214448118.png)

![image-20260326214456518](./LeetCode--代码随想录(双指针).assets/image-20260326214456518.png)

那么删除倒数第N个结点：

![image-20260326214511795](./LeetCode--代码随想录(双指针).assets/image-20260326214511795.png)

## 代码

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        // 新建虚拟头结点
        ListNode dummyHead = new ListNode(0, head);
        ListNode fast = dummyHead;
        ListNode slow = dummyHead;
        // 让fast指针先走N+1步
        for(int i=0; i<n+1; i++){
            fast = fast.next;
        }
        // fast指针没走到最后时，slow和fast一起走
        while(fast != null){
            fast = fast.next;
            slow = slow.next;
        }
        // 删除slow指针指向的下一个结点
        slow.next = slow.next.next;
        // 返回头结点
        return dummyHead.next;

    }
}
```



# 面试题 02.07. 链表相交

## 题目描述

给你两个单链表的头节点 `headA` 和 `headB` ，请你找出并返回两个单链表相交的起始节点。如果两个链表没有交点，返回 `null` 。

图示两个链表在节点 `c1` 开始相交**：**

[![img](./LeetCode--代码随想录(双指针).assets/160_statement.png)](https://assets.leetcode.cn/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)

题目数据 **保证** 整个链式结构中不存在环。

**注意**，函数返回结果后，链表必须 **保持其原始结构** 。

 

**示例 1：**

[![img](./LeetCode--代码随想录(双指针).assets/160_example_1.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_1.png)

```
输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
输出：Intersected at '8'
解释：相交节点的值为 8 （注意，如果两个链表相交则不能为 0）。
从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。
在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
```

**示例 2：**

[![img](./LeetCode--代码随想录(双指针).assets/160_example_2.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_2.png)

```
输入：intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
输出：Intersected at '2'
解释：相交节点的值为 2 （注意，如果两个链表相交则不能为 0）。
从各自的表头开始算起，链表 A 为 [0,9,1,2,4]，链表 B 为 [3,2,4]。
在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。
```

**示例 3：**

[![img](./LeetCode--代码随想录(双指针).assets/160_example_3.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_3.png)

```
输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
输出：null
解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。
由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
这两个链表不相交，因此返回 null 。
```

 

**提示：**

- `listA` 中节点数目为 `m`
- `listB` 中节点数目为 `n`
- `0 <= m, n <= 3 * 104`
- `1 <= Node.val <= 105`
- `0 <= skipA <= m`
- `0 <= skipB <= n`
- 如果 `listA` 和 `listB` 没有交点，`intersectVal` 为 `0`
- 如果 `listA` 和 `listB` 有交点，`intersectVal == listA[skipA + 1] == listB[skipB + 1]`

 

**进阶：**你能否设计一个时间复杂度 `O(n)` 、仅用 `O(1)` 内存的解决方案？

## 解题思路

### 方法一

对齐两个链表的尾部再开始遍历：

![image-20260327210630688](./LeetCode--代码随想录(双指针).assets/image-20260327210630688.png)

### 方法二

通过1/2次交换循环找到共同结点：

![image-20260327210738296](./LeetCode--代码随想录(双指针).assets/image-20260327210738296.png)

![image-20260327210812203](./LeetCode--代码随想录(双指针).assets/image-20260327210812203.png)

![image-20260327210816045](./LeetCode--代码随想录(双指针).assets/image-20260327210816045.png)

## 代码

### 方法一

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode curA = headA;
        ListNode curB = headB;
        
        // 不相等就一直走
        while(curA != curB){
            if(curA == null){
                curA = headB;
            } else{
                curA = curA.next;
            }
            if(curB == null){
                curB = headA;
            } else{
            curB = curB.next;
            }
        }
        return curA;
    }
}
```



### 方法二

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode curA = headA;
        ListNode curB = headB;
        
        // 不相等就一直走
        while(curA != curB){
            if(curA == null){
                curA = headB;
            } else{
                curA = curA.next;
            }
            if(curB == null){
                curB = headA;
            } else{
            curB = curB.next;
            }
        }
        return curA;
    }
}
```





# 142.环形链表 II

## 题目描述

给定一个链表的头节点  `head` ，返回链表开始入环的第一个节点。 *如果链表无环，则返回 `null`。*

如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（**索引从 0 开始**）。如果 `pos` 是 `-1`，则在该链表中没有环。**注意：`pos` 不作为参数进行传递**，仅仅是为了标识链表的实际情况。

**不允许修改** 链表。

 

**示例 1：**

![img](./LeetCode--代码随想录(双指针).assets/circularlinkedlist.png)

```
输入：head = [3,2,0,-4], pos = 1
输出：返回索引为 1 的链表节点
解释：链表中有一个环，其尾部连接到第二个节点。
```

**示例 2：**

![img](./LeetCode--代码随想录(双指针).assets/circularlinkedlist_test2.png)

```
输入：head = [1,2], pos = 0
输出：返回索引为 0 的链表节点
解释：链表中有一个环，其尾部连接到第一个节点。
```

**示例 3：**

![img](./LeetCode--代码随想录(双指针).assets/circularlinkedlist_test3.png)

```
输入：head = [1], pos = -1
输出：返回 null
解释：链表中没有环。
```

 

**提示：**

- 链表中节点的数目范围在范围 `[0, 104]` 内
- `-105 <= Node.val <= 105`
- `pos` 的值为 `-1` 或者链表中的一个有效索引

 

**进阶：**你是否可以使用 `O(1)` 空间解决此题？

## 解题思路

1、通过快慢指针确定是否存在环。（快指针：2步/次；慢指针：1步/次）

![image-20260328202822335](./LeetCode--代码随想录(双指针).assets/image-20260328202822335.png)

2、通过相遇点计算出x、y、z的关系式

![image-20260328202827067](./LeetCode--代码随想录(双指针).assets/image-20260328202827067.png)

说明：因为快指针在环内走两圈，慢指针才走完一圈环，所以快慢指针必定在慢指针进入环内的**第一圈**相遇！即慢指针相遇时走了`x+y`步！

![image-20260328202831117](./LeetCode--代码随想录(双指针).assets/image-20260328202831117.png)

3、给定一个指针`index1`指向头结点，一个指针`index2`指向快慢指针环内相遇结点。

![image-20260328202835325](./LeetCode--代码随想录(双指针).assets/image-20260328202835325.png)

4、通过`x=(n-1)(y+z)+z`等式，我们先让`index1/index2`各走`(n-1)(y+z)`步。

![image-20260328202851450](./LeetCode--代码随想录(双指针).assets/image-20260328202851450.png)

5、再让`index1/index2`各走`z`步，他们就会在环形入口相遇。

![image-20260328202918483](./LeetCode--代码随想录(双指针).assets/image-20260328202918483.png)

## 代码

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode detectCycle(ListNode head) {
        // 给定快慢指针
        ListNode fast = head;
        ListNode slow = head;
        while(fast != null && fast.next != null){
            // 快指针一次走两步
            fast = fast.next.next;
            // 慢指针一次走一步
            slow = slow.next;
            if(fast == slow){// 有环
                // 给定index1和index2指针，分别指向head和相遇点
                ListNode index1 = head;
                ListNode index2 = fast;
                while(index1 != index2){
                    index1 = index1.next;
                    index2 = index2.next;
                }
                // 返回环形入口
                return index1;
            }
        }
        // 无环
        return null;
    }
}
```

# 15.三数之和

## 题目描述

给你一个整数数组 `nums` ，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j`、`i != k` 且 `j != k` ，同时还满足 `nums[i] + nums[j] + nums[k] == 0` 。请你返回所有和为 `0` 且不重复的三元组。

**注意：**答案中不可以包含重复的三元组。

 

 

**示例 1：**

```
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
解释：
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。
不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
注意，输出的顺序和三元组的顺序并不重要。
```

**示例 2：**

```
输入：nums = [0,1,1]
输出：[]
解释：唯一可能的三元组和不为 0 。
```

**示例 3：**

```
输入：nums = [0,0,0]
输出：[[0,0,0]]
解释：唯一可能的三元组和为 0 。
```

 

**提示：**

- `3 <= nums.length <= 3000`
- `-105 <= nums[i] <= 105`

## 题解

**双指针法**是不会漏解，具有高效剪枝效率的查找方法。

在使用前需要将给定**数组**进行从小到大**排序**。

![image-20260406112509942](./LeetCode--代码随想录(双指针).assets/image-20260406112509942.png)

![image-20260406112514705](./LeetCode--代码随想录(双指针).assets/image-20260406112514705.png)

## 代码

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        // 将数组有序排列
        Arrays.sort(nums);
        // 最终要返回的结果组
        List<List<Integer>> result = new ArrayList<>();

        // 固定一端i，左右指针遍历三元组
        for(int i=0; i<nums.length-2; i++){
            // 边界条件判断
            if(nums[i]>0) break;
            // 对下标i对应元素进行去重
            if(i>0 && nums[i]==nums[i-1]) continue;
            // 左指针
            int left = i+1;
            // 右指针
            int right = nums.length-1;
            // 双指针相向运动
            while(left < right){
                int sum = nums[i]+nums[left]+nums[right];
                // 当和大于0，right左移
                if(sum > 0){
                    right--;
                }else if(sum < 0){// 当和小于0，left右移
                    left++;
                } else{
                    result.add(Arrays.asList(nums[i],nums[left],nums[right]));

                    // 对left和right进行去重
                    while(left < right && nums[left]==nums[left+1]) left++;
                    while(left < right && nums[right]==nums[right-1]) right--;

                    // 将一组正确的解过滤
                    left++;
                    right--;
                }
            }
        }

        return result;

    }
}
```

# 18.四数之和

## 题目描述

给你一个由 `n` 个整数组成的数组 `nums` ，和一个目标值 `target` 。请你找出并返回满足下述全部条件且**不重复**的四元组 `[nums[a], nums[b], nums[c], nums[d]]` （若两个四元组元素一一对应，则认为两个四元组重复）：

- `0 <= a, b, c, d < n`
- `a`、`b`、`c` 和 `d` **互不相同**
- `nums[a] + nums[b] + nums[c] + nums[d] == target`

你可以按 **任意顺序** 返回答案 。

 

**示例 1：**

```
输入：nums = [1,0,-1,0,-2,2], target = 0
输出：[[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]
```

**示例 2：**

```
输入：nums = [2,2,2,2,2], target = 8
输出：[[2,2,2,2]]
```

 

**提示：**

- `1 <= nums.length <= 200`
- `-10^9 <= nums[i] <= 10^9`
- `-10^9 <= target <= 10^9`

## 题解

尝试复刻三数之和，使用$O(n^2)$时间复杂度解决，失败：

![image-20260406153405371](./LeetCode--代码随想录(双指针).assets/image-20260406153405371.png)

固定两个指针，再用另外两个指针进行扫描$O(n^3)$时间复杂度解决

![image-20260406153450551](./LeetCode--代码随想录(双指针).assets/image-20260406153450551.png)

![image-20260406153455978](./LeetCode--代码随想录(双指针).assets/image-20260406153455978.png)

## 代码

错误解法：

```java
class Solution {
    // 这种双指针写法过于贪心，无法适用于4Sum问题
    public List<List<Integer>> fourSum(int[] nums, int target) {
        // 最终要返回的结果列表
        List<List<Integer>> result = new ArrayList<>();
        // 对数组进行排序
        Arrays.sort(nums);
        // 给定左右两个外围指针
        int leftOut = 0;
        int rightOut = nums.length - 1;
        // 两对双指针遍历数组
        while(leftOut < rightOut-2){
            // 边界条件判断
            if(nums[leftOut]>target) break;
            // 对leftOut和rightOut进行剪枝
            if(leftOut>0 && nums[leftOut]==nums[leftOut-1]){
                leftOut++;
                continue;
            }
            if(rightOut<nums.length-1 && nums[rightOut]==nums[rightOut+1]){
                rightOut--;
                continue;
            }
            // 初始化左右两个内部指针
            int leftIn = leftOut+1;
            int rightIn = rightOut-1;
            while(leftIn < rightIn){
                int sum = nums[leftIn]+nums[leftOut]+nums[rightOut]+nums[rightIn];
                if(sum > target){
                    rightIn--;
                    continue;
                } else if(sum < target){
                    leftIn++;
                    continue;
                } else{
                    // 加入结果四元组
                    result.add(Arrays.asList(nums[leftIn],nums[leftOut],nums[rightIn],nums[rightOut]));
                    // 对leftIn和rightIn去重剪枝
                    while(leftIn<rightIn && nums[leftIn]==nums[leftIn+1]) leftIn++;
                    while(leftIn<rightIn && nums[rightIn]==nums[rightIn-1]) rightIn--;
                    
                    // 过滤掉一组正确解
                    rightIn--;
                    leftIn++;
                }
            }
            if(nums[rightOut]+nums[leftOut]>=target){
                rightOut--;
            } else{
                leftOut++;
            }
            

        }

        return result;
    }
}
```

正确解法：

```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        // 最终要返回的列表
        List<List<Integer>> result = new ArrayList<>();
        // 对数组进行排序
        Arrays.sort(nums);

        // 固定前两个数，用剩余部分进行双指针扫描
        for(int i=0; i<nums.length-3; i++){
            // 边界条件判断，防止大数溢出
            long min = (long) nums[i]+nums[i+1]+nums[i+2]+nums[i+3];
            if(min > target) break;
            long max = (long) nums[i]+nums[nums.length-1]+nums[nums.length-2]+nums[nums.length-3];
            if(max < target) continue;
            // 对i去重
            if(i>0 && nums[i]==nums[i-1]) continue;
            // 固定第二个数
            for(int j=i+1; j<nums.length-2; j++){
                // 对j去重
                if(j>i+1 && nums[j]==nums[j-1]) continue;
                // 给定双指针
                int left = j+1;
                int right = nums.length-1;
                while(left<right){
                    // 防止大数溢出
                    long sum = (long)nums[i]+nums[j]+nums[left]+nums[right];
                    if(sum > target){
                        right--;
                        continue;
                    } else if(sum < target){
                        left++;
                        continue;
                    } else{
                        result.add(Arrays.asList(nums[i],nums[j],nums[left],nums[right]));
                        // 对left和right去重
                        while(left<right && nums[left]==nums[left+1]) left++;
                        while(left<right && nums[right]==nums[right-1]) right--;
                        // 过滤一组正确解
                        left++;
                        right--;
                    }
                }
            }
        }
        return result;
    }
}
```
