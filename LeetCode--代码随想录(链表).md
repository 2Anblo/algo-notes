# LeetCode--代码随想录(链表)

## 203.移除链表元素

### 题目描述

给你一个链表的头节点 `head` 和一个整数 `val` ，请你删除链表中所有满足 `Node.val == val` 的节点，并返回 **新的头节点** 。

 

**示例 1：**

![img](./LeetCode--代码随想录(链表).assets/removelinked-list.jpg)

```
输入：head = [1,2,6,3,4,5,6], val = 6
输出：[1,2,3,4,5]
```

**示例 2：**

```
输入：head = [], val = 1
输出：[]
```

**示例 3：**

```
输入：head = [7,7,7,7], val = 7
输出：[]
```

 

**提示：**

- 列表中的节点数目在范围 `[0, 104]` 内
- `1 <= Node.val <= 50`
- `0 <= val <= 50`

### 解题思路

#### 思路1

分类讨论`删除头结点`和`删除其它结点`的情况。

![image-20260324193721199](./LeetCode--代码随想录(链表).assets/image-20260324193721199.png)

![image-20260324193730234](./LeetCode--代码随想录(链表).assets/image-20260324193730234.png)

#### 思路2

引入一个虚拟头结点，即可避开讨论头结点的情况。

![image-20260324194358815](./LeetCode--代码随想录(链表).assets/image-20260324194358815.png)

### 代码

**方法一：**

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
    public ListNode removeElements(ListNode head, int val) {
        // 从头结点开始遍历
        ListNode currentNode = head;

        // 若要删除的是头结点
        while(head != null && head.val == val){
            head = head.next;
            currentNode = head;
        }

        // 若要删除其它结点
        while(currentNode != null && currentNode.next != null){
            if(currentNode.next.val == val){
                currentNode.next = currentNode.next.next;
            } else{
            currentNode = currentNode.next;
            }

        }

        return head;
    }
}
```

**方法二：**

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
    public ListNode removeElements(ListNode head, int val) {
        // 创建一个虚拟头结点，next指针指向head结点
        ListNode dummyHead = new ListNode(0,head);
        // 从虚拟头结点开始遍历
        ListNode currentNode = dummyHead;

        // 若要删除其它结点
        while(currentNode != null && currentNode.next != null){
            if(currentNode.next.val == val){
                currentNode.next = currentNode.next.next;
            } else{
            currentNode = currentNode.next;
            }

        }
        // 结果返回真实头结点
        return dummyHead.next;
    }
}
```

