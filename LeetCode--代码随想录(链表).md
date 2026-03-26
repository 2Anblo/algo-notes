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

## 707.设计链表

### 题目描述

你可以选择使用单链表或者双链表，设计并实现自己的链表。

单链表中的节点应该具备两个属性：`val` 和 `next` 。`val` 是当前节点的值，`next` 是指向下一个节点的指针/引用。

如果是双向链表，则还需要属性 `prev` 以指示链表中的上一个节点。假设链表中的所有节点下标从 **0** 开始。

实现 `MyLinkedList` 类：

- `MyLinkedList()` 初始化 `MyLinkedList` 对象。
- `int get(int index)` 获取链表中下标为 `index` 的节点的值。如果下标无效，则返回 `-1` 。
- `void addAtHead(int val)` 将一个值为 `val` 的节点插入到链表中第一个元素之前。在插入完成后，新节点会成为链表的第一个节点。
- `void addAtTail(int val)` 将一个值为 `val` 的节点追加到链表中作为链表的最后一个元素。
- `void addAtIndex(int index, int val)` 将一个值为 `val` 的节点插入到链表中下标为 `index` 的节点之前。如果 `index` 等于链表的长度，那么该节点会被追加到链表的末尾。如果 `index` 比长度更大，该节点将 **不会插入** 到链表中。
- `void deleteAtIndex(int index)` 如果下标有效，则删除链表中下标为 `index` 的节点。

 

**示例：**

```
输入
["MyLinkedList", "addAtHead", "addAtTail", "addAtIndex", "get", "deleteAtIndex", "get"]
[[], [1], [3], [1, 2], [1], [1], [1]]
输出
[null, null, null, null, 2, null, 3]

解释
MyLinkedList myLinkedList = new MyLinkedList();
myLinkedList.addAtHead(1);
myLinkedList.addAtTail(3);
myLinkedList.addAtIndex(1, 2);    // 链表变为 1->2->3
myLinkedList.get(1);              // 返回 2
myLinkedList.deleteAtIndex(1);    // 现在，链表变为 1->3
myLinkedList.get(1);              // 返回 3
```

 

**提示：**

- `0 <= index, val <= 1000`
- 请不要使用内置的 LinkedList 库。
- 调用 `get`、`addAtHead`、`addAtTail`、`addAtIndex` 和 `deleteAtIndex` 的次数不超过 `2000` 。

### 代码

```java
// 定义一个链表节点
class MyLinkedNode {
    public int val;
    public MyLinkedNode next;

    public MyLinkedNode(){
        this.next = null;
    }
    
    public MyLinkedNode(int val){
        this.val = val;
        this.next = null;
    }
    
    public MyLinkedNode(int val, MyLinkedNode next){
        this.next = next;
        this.val = val;
    }

}

class MyLinkedList {

    public MyLinkedNode head;

    public MyLinkedList() {

    }
    
    public MyLinkedList(MyLinkedNode head) {
        this.head = head;
    }

    public int getLength(){
        MyLinkedNode current = head;
        if(head == null){
            return 0;
        }
        int count = 1;
        while(current.next != null){
            current = current.next;
            count++;
        }
        return count;
    }
    
    public int get(int index) {
        MyLinkedNode current = head;
        int count = 0;
        if(head == null){
            return -1;
        }
        while(current.next != null && count != index){
            current = current.next;
            count++;
        }
        if(count == index){
            return current.val;
        } else{
            return -1;
        }

    }
    
    public void addAtHead(int val) {
        MyLinkedNode node = new MyLinkedNode(val,head);
        head = node;
    }
    
    public void addAtTail(int val) {
        MyLinkedNode node = new MyLinkedNode(val);
        MyLinkedNode current = head;
        if(head == null){
            addAtHead(val);
        } else{
            while(current.next != null){
                current = current.next;
            }
            current.next = node;
        }

    }
    
    public void addAtIndex(int index, int val) {
        MyLinkedNode current = head;
        if(index == 0){
            // 插入到第一个元素之前
            addAtHead(val);
        } else if(index == getLength()){
            // 插入到最后一个元素之后
            addAtTail(val);
        }else  if(index < getLength()){
            // 插入到其他元素之前
            MyLinkedNode node = new MyLinkedNode(val);
            int count = 0;
            while(current.next != null && count != index - 1){
                current = current.next;
                count++;
            }
            if(count == index - 1){
                node.next = current.next;
                current.next = node;
            }
        }
    }
    
    public void deleteAtIndex(int index) {
        MyLinkedNode dummyHead = new MyLinkedNode(0,head);
        MyLinkedNode current = dummyHead;
        int count = 0;
        while(index < getLength() && current.next != null){
            if(count != index){
                count++;
                current = current.next;
            } else{
                current.next = current.next.next;
                break;
            }
        }
        head = dummyHead.next;
    }
}

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList obj = new MyLinkedList();
 * int param_1 = obj.get(index);
 * obj.addAtHead(val);
 * obj.addAtTail(val);
 * obj.addAtIndex(index,val);
 * obj.deleteAtIndex(index);
 */
```

## 206.反转链表

### 题目描述

给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表。

 

**示例 1：**

![img](./LeetCode--代码随想录(链表).assets/rev1ex1.jpg)

```
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
```

**示例 2：**

![img](./LeetCode--代码随想录(链表).assets/rev1ex2.jpg)

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

### 解题思路

#### 双指针法

![image-20260325203946407](./LeetCode--代码随想录(链表).assets/image-20260325203946407.png)

![image-20260325203959618](./LeetCode--代码随想录(链表).assets/image-20260325203959618.png)



### 代码

#### 双指针法

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

#### 递归法

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

    public ListNode reverse(ListNode prev, ListNode cur){
        if(cur == null){
            //已经遍历到最后结点，返回prev作为新的头结点
            return prev;
        }
        // 保存下一个结点
        ListNode next = cur.next;
        // 反转
        cur.next = prev;
        // 递归
        return reverse(cur, next);
    }

    public ListNode reverseList(ListNode head) {
        return reverse(null, head);
    }
}
```

## 24.两两交换链表中的节点

### 题目描述

给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。

 

**示例 1：**

![img](./LeetCode--代码随想录(链表).assets/swap_ex1.jpg)

```
输入：head = [1,2,3,4]
输出：[2,1,4,3]
```

**示例 2：**

```
输入：head = []
输出：[]
```

**示例 3：**

```
输入：head = [1]
输出：[1]
```

 

**提示：**

- 链表中节点的数目在范围 `[0, 100]` 内
- `0 <= Node.val <= 100`

 

### 解题思路

注意`prev`指针要正确指向下一组`nextPair`

![image-20260325214922500](./LeetCode--代码随想录(链表).assets/image-20260325214922500.png)

![image-20260325214927969](./LeetCode--代码随想录(链表).assets/image-20260325214927969.png)

### 代码

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
    public ListNode swapPairs(ListNode head) {
        ListNode cur = head;
        // 边缘条件判断
        if(head == null || cur.next == null){
            return head;
        }
        // 重置头结点
        head = cur.next;
        // 记录下一组
        ListNode nextPair = cur.next.next;
        // 记录prev指针
        ListNode prev = cur;
        cur.next.next = cur;
        cur.next = nextPair;
        // 跳至下一组
        cur = nextPair;

        // 若组不成一组不需要反转
        while(nextPair != null && cur.next != null){
            prev.next = cur.next;
            prev = cur;
            nextPair = cur.next.next;
            cur.next.next = cur;
            cur.next = nextPair;
            cur = nextPair;
        }

        return head;
    }
}
```

## 19.删除链表的倒数第 N 个结点

### 题目描述

给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。

 

**示例 1：**

![img](./LeetCode--代码随想录(链表).assets/remove_ex1.jpg)

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

### 解题思路

使用快慢指针和虚拟头结点来实现一趟扫描删除倒数第N个结点。

以删除倒数第2个结点为例：

![image-20260326214448118](./LeetCode--代码随想录(链表).assets/image-20260326214448118.png)

![image-20260326214456518](./LeetCode--代码随想录(链表).assets/image-20260326214456518.png)

那么删除倒数第N个结点：

![image-20260326214511795](./LeetCode--代码随想录(链表).assets/image-20260326214511795.png)

### 代码

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

