# 1、二叉树的深度优先遍历（DFS）

## 中序遍历

给定一个二叉树的根节点 `root` ，返回 *它的 **中序** 遍历* 。

 

**示例 1：**

![img](./LeetCode--代码随想录(二叉树).assets/inorder_1.jpg)

```
输入：root = [1,null,2,3]
输出：[1,3,2]
```

**示例 2：**

```
输入：root = []
输出：[]
```

**示例 3：**

```
输入：root = [1]
输出：[1]
```

 

**提示：**

- 树中节点数目在范围 `[0, 100]` 内
- `-100 <= Node.val <= 100`

### 图解

![image-20260428211708129](./LeetCode--代码随想录(二叉树).assets/image-20260428211708129.png)

### 递归法

只要让取中间节点值在中间即可。（注意终止条件）

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        traversal(result, root);
        return result;
    }

    public void traversal(List<Integer> result, TreeNode node){
        // 递归终止条件
        if(node == null){
            return;
        }
        // 中 左 右 中序遍历
        traversal(result, node.left);
        result.add(node.val);
        traversal(result, node.right);
    }
}
```

### 迭代法

这里的迭代法借助**栈(Stack)**来实现，由于其后进先出的特性，我们在做中序遍历时，要以右→中→左的顺序倒序进栈。（在栈中我们使用`null`作为出栈标志位）

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        Stack<TreeNode> st = new Stack<>();
        if(root != null) st.push(root);
        while(!st.isEmpty()){
            TreeNode cur = st.peek();
            if(cur != null){
               // 先弹出中间节点 让右节点进栈
               st.pop();
               // 右→中→左
               if(cur.right != null) st.push(cur.right);
               // 中间节点二次进栈需要加上null标志
               st.push(cur);
               st.push(null);
               if(cur.left != null) st.push(cur.left);
               
            } else{
                // 将空节点弹出
                st.pop();
                result.add(st.pop().val);
            }
        }
        return result;
    }
}
```

## 前序遍历

给你二叉树的根节点 `root` ，返回它节点值的 **前序** 遍历。

 

**示例 1：**

**输入：**root = [1,null,2,3]

**输出：**[1,2,3]

**解释：**

![img](./LeetCode--代码随想录(二叉树).assets/screenshot-2024-08-29-202743.png)

**示例 2：**

**输入：**root = [1,2,3,4,5,null,8,null,null,6,7,9]

**输出：**[1,2,4,5,6,7,3,8,9]

**解释：**

![img](./LeetCode--代码随想录(二叉树).assets/tree_2.png)

**示例 3：**

**输入：**root = []

**输出：**[]

**示例 4：**

**输入：**root = [1]

**输出：**[1]

 

**提示：**

- 树中节点数目在范围 `[0, 100]` 内
- `-100 <= Node.val <= 100`

 

**进阶：**递归算法很简单，你可以通过迭代算法完成吗？

### 图解

![image-20260428212505242](./LeetCode--代码随想录(二叉树).assets/image-20260428212505242.png)

### 递归法

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        // 递归前序遍历
        traversal(result, root);
        return result;
    }

    public void traversal(List<Integer> result, TreeNode node){
        if(node == null){
            return;
        }
        // 中 左 右 前序遍历
        result.add(node.val);
        traversal(result, node.left);
        traversal(result, node.right);
    }
}
```

### 迭代法

同样用`null`做标志位，右→左→中压入栈。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        // 迭代前序遍历
        if(root == null){
            return result;
        }
        Stack<TreeNode> st = new Stack<>();
        st.push(root);
        while(!st.isEmpty()){
            // 先取出中间节点
            TreeNode node = st.peek();
            if(node != null){
                // 将中间节点先弹出 让左右节点先进栈
                st.pop();
                // 先右后左
                if(node.right != null) st.push(node.right);
                if(node.left != null) st.push(node.left);
                // 重新压入中间节点 并加上null标志
                st.push(node);
                st.push(null);
            } else{
                // 弹出null
                st.pop();
                // 中间节点入队
                result.add(st.pop().val);
            }
        }
        return result;
    }

}
```

## 后序遍历

给你一棵二叉树的根节点 `root` ，返回其节点值的 **后序遍历** 。

 

**示例 1：**

**输入：**root = [1,null,2,3]

**输出：**[3,2,1]

**解释：**

![img](./LeetCode--代码随想录(二叉树).assets/screenshot-2024-08-29-202743-1777382798554-7.png)

**示例 2：**

**输入：**root = [1,2,3,4,5,null,8,null,null,6,7,9]

**输出：**[4,6,7,5,2,9,8,3,1]

**解释：**

![img](./LeetCode--代码随想录(二叉树).assets/tree_2-1777382798555-9.png)

**示例 3：**

**输入：**root = []

**输出：**[]

**示例 4：**

**输入：**root = [1]

**输出：**[1]

 

**提示：**

- 树中节点的数目在范围 `[0, 100]` 内
- `-100 <= Node.val <= 100`

 

**进阶：**递归算法很简单，你可以通过迭代算法完成吗？

### 图解

![image-20260428212648980](./LeetCode--代码随想录(二叉树).assets/image-20260428212648980.png)

### 递归法

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        traversal(result, root);
        return result;
    }

    public void traversal(List<Integer> result, TreeNode node){
        // 递归终止条件
        if(node == null){
            return;
        }
        // 左 右 中 后序遍历
        traversal(result, node.left);
        traversal(result, node.right);
        result.add(node.val);
    }
}
```

### 迭代法

同样用`null`做标志位，中→右→左压入栈。

![image-20260428214427839](./LeetCode--代码随想录(二叉树).assets/image-20260428214427839.png)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if(root == null) return result;
        Stack<TreeNode> st = new Stack<>();
        st.push(root);
        while(!st.isEmpty()){
           TreeNode node = st.peek();
           // 若栈顶节点不为空
           if(node != null){
              // 为中间节点加上弹出标志null
              st.push(null);
              if(node.right != null) st.push(node.right);
              if(node.left != null) st.push(node.left);
           }
           else{
            // 栈顶节点为空 则达到入队条件
            st.pop(); //弹出空节点
            result.add(st.pop().val);
           }
        }
        return result;
    }
}
```

# 2、二叉树的层序遍历（BFS）

## 102.二叉树的层序遍历

给你二叉树的根节点 `root` ，返回其节点值的 **层序遍历** 。 （即逐层地，从左到右访问所有节点）。

 

**示例 1：**

![img](./LeetCode--代码随想录(二叉树).assets/tree1.jpg)

```
输入：root = [3,9,20,null,null,15,7]
输出：[[3],[9,20],[15,7]]
```

**示例 2：**

```
输入：root = [1]
输出：[[1]]
```

**示例 3：**

```
输入：root = []
输出：[]
```

 

**提示：**

- 树中节点数目在范围 `[0, 2000]` 内
- `-1000 <= Node.val <= 1000`

### 图解

![image-20260429214447587](./LeetCode--代码随想录(二叉树).assets/image-20260429214447587.png) 

![image-20260429214452889](./LeetCode--代码随想录(二叉树).assets/image-20260429214452889.png)

### 递归法

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
        // 结果列表
    public List<List<Integer>> result = new ArrayList<>();

    public List<List<Integer>> levelOrder(TreeNode root) {
        traverseLevel(root,0);
        return result;
    }

    void traverseLevel(TreeNode node, int deepth){
        // 遇到空结点返回
        if(node == null) return;
        deepth++;
        // 给每一层指定一个列表
        if(deepth > result.size()){
            List<Integer> list = new ArrayList<>();
            result.add(list);
        }
        // 放进结果列表
        result.get(deepth - 1).add(node.val);
        // 将孩子纳入递归
        traverseLevel(node.left, deepth);
        traverseLevel(node.right, deepth);
    }
}
```

### 迭代法

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        // 结果列表
        List<List<Integer>> result = new ArrayList<>();
        if(root == null) return result;
        // 借用队列
        Queue<TreeNode> que = new ArrayDeque<>();
        que.add(root);
        while(!que.isEmpty()){
            int size = que.size();
            List<Integer> list = new ArrayList<>();
            // 每次循环队列中为同一层结点
            for(int i=0; i<size; i++){
                // 将一层中的值打包进一个list
                TreeNode node = que.poll();
                list.add(node.val);
                // 并且将他们的孩子入队
                if(node.left != null) que.add(node.left);
                if(node.right != null) que.add(node.right);
            }
            // 将这一层放入结果队列
            result.add(list);
        }
        return result;
    }
}
```

## 107.二叉树的层序遍历II

给你二叉树的根节点 `root` ，返回其节点值 **自底向上的层序遍历** 。 （即按从叶子节点所在层到根节点所在的层，逐层从左向右遍历）

 

**示例 1：**

![img](./LeetCode--代码随想录(二叉树).assets/tree1-1777470389728-3.jpg)

```
输入：root = [3,9,20,null,null,15,7]
输出：[[15,7],[9,20],[3]]
```

**示例 2：**

```
输入：root = [1]
输出：[[1]]
```

**示例 3：**

```
输入：root = []
输出：[]
```

 

**提示：**

- 树中节点数目在范围 `[0, 2000]` 内
- `-1000 <= Node.val <= 1000`

### 代码

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        // 迭代法实现遍历
        List<List<Integer>> result = new ArrayList<>();
        if(root == null) return result;
        Queue<TreeNode> que = new ArrayDeque<>();
        que.add(root);
        while(!que.isEmpty()){
            List<Integer> list = new ArrayList<>();
            int size = que.size();
            for(int i=0; i<size; i++){
                TreeNode node = que.poll();
                list.add(node.val);
                if(node.left != null) que.add(node.left);
                if(node.right != null) que.add(node.right);
            }

            result.add(list);
        }
        // 最后翻转即可
        Collections.reverse(result);
        return result;
    }
}
```

## 199. 二叉树的右视图

给定一个二叉树的 **根节点** `root`，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

 

**示例 1：**

**输入：**root = [1,2,3,null,5,null,4]

**输出：**[1,3,4]

**解释：**

![img](./LeetCode--代码随想录(二叉树).assets/tmpd5jn43fs-1.png)

**示例 2：**

**输入：**root = [1,2,3,4,null,null,null,5]

**输出：**[1,3,4,5]

**解释：**

![img](./LeetCode--代码随想录(二叉树).assets/tmpkpe40xeh-1.png)

**示例 3：**

**输入：**root = [1,null,3]

**输出：**[1,3]

**示例 4：**

**输入：**root = []

**输出：**[]

 

**提示:**

- 二叉树的节点个数的范围是 `[0,100]`
- `-100 <= Node.val <= 100` 

### 代码

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        Queue<TreeNode> que = new ArrayDeque<>();
        if(root == null) return result;
        que.add(root);
        while(!que.isEmpty()){
            int size = que.size();
            result.add(que.peek().val);
            for(int i=0; i<size; i++){
                TreeNode node = que.poll();
                if(node.right != null) que.add(node.right);
                if(node.left != null) que.add(node.left);
            }
        }
        return result;
    }
}
```

## 637.二叉树的层平均值

给定一个非空二叉树的根节点 `root` , 以数组的形式返回每一层节点的平均值。与实际答案相差 `10-5` 以内的答案可以被接受。

 

**示例 1：**

![img](./LeetCode--代码随想录(二叉树).assets/avg1-tree.jpg)

```
输入：root = [3,9,20,null,null,15,7]
输出：[3.00000,14.50000,11.00000]
解释：第 0 层的平均值为 3,第 1 层的平均值为 14.5,第 2 层的平均值为 11 。
因此返回 [3, 14.5, 11] 。
```

**示例 2:**

![img](./LeetCode--代码随想录(二叉树).assets/avg2-tree.jpg)

```
输入：root = [3,9,20,15,7]
输出：[3.00000,14.50000,11.00000]
```

 

**提示：**



- 树中节点数量在 `[1, 10^4]` 范围内
- `-231 <= Node.val <= 231 - 1`

### 代码

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<Double> averageOfLevels(TreeNode root) {
        List<Double> result = new ArrayList<>();
        if(root == null) return result;
        Queue<TreeNode> que = new ArrayDeque<>();
        que.add(root);
        while(!que.isEmpty()){
            int size = que.size();
            double sum = 0;
            for(int i=0; i<size; i++){
                TreeNode node = que.poll();
                sum += node.val;
                if(node.left != null) que.add(node.left);
                if(node.right != null) que.add(node.right);
            }
            result.add(sum/size);
        }
        return result;
    }
}
```

## 429.N叉树的层序遍历

给定一个 N 叉树，返回其节点值的*层序遍历*。（即从左到右，逐层遍历）。

树的序列化输入是用层序遍历，每组子节点都由 null 值分隔（参见示例）。

 

**示例 1：**

![img](./LeetCode--代码随想录(二叉树).assets/narytreeexample.png)

```
输入：root = [1,null,3,2,4,null,5,6]
输出：[[1],[3,2,4],[5,6]]
```

**示例 2：**

![img](./LeetCode--代码随想录(二叉树).assets/sample_4_964.png)

```
输入：root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
输出：[[1],[2,3,4,5],[6,7,8,9,10],[11,12,13],[14]]
```

 

**提示：**

- 树的高度不会超过 `1000`
- 树的节点总数在 `[0, 10^4]` 之间

### 代码

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> children;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, List<Node> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
    public List<List<Integer>> levelOrder(Node root) {
        List<List<Integer>> result = new ArrayList<>();
        Queue<Node> que = new ArrayDeque<>();
        if(root == null) return result;
        que.add(root);
        while(!que.isEmpty()){
            int size = que.size();
            List<Integer> levelList = new ArrayList<>();
            for(int i=0; i<size; i++){
                Node node = que.poll();
                levelList.add(node.val);
                if(node.children != null){
                    for(Node n : node.children){
                        que.add(n);
                    }
                }
            }
            result.add(levelList);
        }
        return result;
    }
}
```

## 515.在每个树行中找最大值

给定一棵二叉树的根节点 `root` ，请找出该二叉树中每一层的最大值。

 

**示例1：**

![img](./LeetCode--代码随想录(二叉树).assets/largest_e1.jpg)

```
输入: root = [1,3,2,5,3,null,9]
输出: [1,3,9]
```

**示例2：**

```
输入: root = [1,2,3]
输出: [1,3]
```

 

**提示：**

- 二叉树的节点个数的范围是 `[0,10^4]`
- `-2^31 <= Node.val <= 2^31 - 1`

### 代码

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<Integer> largestValues(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if(root == null) return result;
        Queue<TreeNode> que = new ArrayDeque<>();
        que.add(root);
        while(!que.isEmpty()){
            int size = que.size();
            int max = que.peek().val;
            for(int i=0; i<size; i++){
                TreeNode node = que.poll();
                if(node.val > max) max = node.val;
                if(node.left != null) que.add(node.left);
                if(node.right != null) que.add(node.right);
            }
            result.add(max);
        }
        return result;
    }
}
```

## 116.填充每个节点的下一个右侧节点指针

给定一个 **完美二叉树** ，其所有叶子节点都在同一层，每个父节点都有两个子节点。二叉树定义如下：

```
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 `NULL`。

初始状态下，所有 next 指针都被设置为 `NULL`。

 

**示例 1：**

![img](./LeetCode--代码随想录(二叉树).assets/116_sample.png)

```
输入：root = [1,2,3,4,5,6,7]
输出：[1,#,2,3,#,4,5,6,7,#]
解释：给定二叉树如图 A 所示，你的函数应该填充它的每个 next 指针，以指向其下一个右侧节点，如图 B 所示。序列化的输出按层序遍历排列，同一层节点由 next 指针连接，'#' 标志着每一层的结束。
```



**示例 2:**

```
输入：root = []
输出：[]
```

 

**提示：**

- 树中节点的数量在 `[0, 212 - 1]` 范围内
- `-1000 <= node.val <= 1000`

 

**进阶：**

- 你只能使用常量级额外空间。
- 使用递归解题也符合要求，本题中递归程序占用的栈空间不算做额外的空间复杂度。

### 代码

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node next;

    public Node() {}
    
    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, Node _left, Node _right, Node _next) {
        val = _val;
        left = _left;
        right = _right;
        next = _next;
    }
};
*/

class Solution {
    public Node connect(Node root) {
        Queue<Node> que = new ArrayDeque<>();
        if(root == null) return root;
        que.add(root);
        while(!que.isEmpty()){
            int size = que.size();
            for(int i=0; i<size; i++){
                Node node = que.poll();
                if(i==size-1)           
                    node.next = null;
                else 
                    node.next = que.peek();

                if(node.left != null) que.add(node.left);
                if(node.right != null) que.add(node.right);
            }
        }
        return root;
    }
}
```

## 117.填充每个节点的下一个右侧节点指针II

给定一个二叉树：

```
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 `NULL` 。

初始状态下，所有 next 指针都被设置为 `NULL` 。

 

**示例 1：**

![img](./LeetCode--代码随想录(二叉树).assets/117_sample.png)

```
输入：root = [1,2,3,4,5,null,7]
输出：[1,#,2,3,#,4,5,7,#]
解释：给定二叉树如图 A 所示，你的函数应该填充它的每个 next 指针，以指向其下一个右侧节点，如图 B 所示。序列化输出按层序遍历顺序（由 next 指针连接），'#' 表示每层的末尾。
```

**示例 2：**

```
输入：root = []
输出：[]
```

 

**提示：**

- 树中的节点数在范围 `[0, 6000]` 内
- `-100 <= Node.val <= 100`

**进阶：**

- 你只能使用常量级额外空间。
- 使用递归解题也符合要求，本题中递归程序的隐式栈空间不计入额外空间复杂度。

### 代码

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node next;

    public Node() {}
    
    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, Node _left, Node _right, Node _next) {
        val = _val;
        left = _left;
        right = _right;
        next = _next;
    }
};
*/

class Solution {
    public Node connect(Node root) {
        Queue<Node> que = new ArrayDeque<>();
        if(root == null) return root;
        que.add(root);
        while(!que.isEmpty()){
            int size = que.size();
            for(int i=0; i<size; i++){
                Node node = que.poll();
                if(i==size-1)           
                    node.next = null;
                else 
                    node.next = que.peek();

                if(node.left != null) que.add(node.left);
                if(node.right != null) que.add(node.right);
            }
        }
        return root;
    }
}
```

## 104.二叉树的最大深度

给定一个二叉树 `root` ，返回其最大深度。

二叉树的 **最大深度** 是指从根节点到最远叶子节点的最长路径上的节点数。

 

**示例 1：**

![img](./LeetCode--代码随想录(二叉树).assets/tmp-tree.jpg)

 

```
输入：root = [3,9,20,null,null,15,7]
输出：3
```

**示例 2：**

```
输入：root = [1,null,2]
输出：2
```

 

**提示：**

- 树中节点的数量在 `[0, 10^4]` 区间内。
- `-100 <= Node.val <= 100`

###  代码

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public int maxDepth(TreeNode root) {
        int depth = 0;
        if(root == null) return depth;
        Queue<TreeNode> que = new ArrayDeque<>();
        que.add(root);
        while(!que.isEmpty()){
            depth++;
            int size = que.size();
            for(int i=0; i<size; i++){
                TreeNode node = que.poll();
                if(node.left != null) que.add(node.left);
                if(node.right != null) que.add(node.right);
            }
        }
        return depth;
    }
}
```



## 111.二叉树的最小深度

给定一个二叉树，找出其最小深度。

最小深度是从根节点到最近叶子节点的最短路径上的节点数量。

**说明：**叶子节点是指没有子节点的节点。

 

**示例 1：**

![img](./LeetCode--代码随想录(二叉树).assets/ex_depth.jpg)

```
输入：root = [3,9,20,null,null,15,7]
输出：2
```

**示例 2：**

```
输入：root = [2,null,3,null,4,null,5,null,6]
输出：5
```

 

**提示：**

- 树中节点数的范围在 `[0, 105]` 内
- `-1000 <= Node.val <= 1000`

### 代码

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public int minDepth(TreeNode root) {
        int depth = 0;
        if(root == null) return depth;
        Queue<TreeNode> que = new ArrayDeque<>();
        que.add(root);
        depth++;
        while(!que.isEmpty()){
            int size = que.size();
            for(int i=0; i<size; i++){
                TreeNode node = que.poll();
                if(node.left == null && node.right == null) return depth;
                if(node.left != null) que.add(node.left);
                if(node.right != null) que.add(node.right);
            }
            depth++;
        }
        return depth;
    }
}
```

# 226.翻转二叉树

## 题目描述

给你一棵二叉树的根节点 `root` ，翻转这棵二叉树，并返回其根节点。

 

**示例 1：**

![img](./LeetCode--代码随想录(二叉树).assets/invert1-tree.jpg)

```
输入：root = [4,2,7,1,3,6,9]
输出：[4,7,2,9,6,3,1]
```

**示例 2：**

![img](./LeetCode--代码随想录(二叉树).assets/invert2-tree.jpg)

```
输入：root = [2,1,3]
输出：[2,3,1]
```

**示例 3：**

```
输入：root = []
输出：[]
```

 

**提示：**

- 树中节点数目范围在 `[0, 100]` 内
- `-100 <= Node.val <= 100`

## 代码

使用层序遍历交换左右节点：

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public TreeNode invertTree(TreeNode root) {
        Queue<TreeNode> que = new ArrayDeque<>();
        if(root == null) return root;
        que.add(root);
        while(!que.isEmpty()){
            int size = que.size();
            for(int i=0; i<size; i++){
                TreeNode node = que.poll();
                TreeNode temp = node.right;
                node.right = node.left;
                node.left = temp;
                if(node.left != null) que.add(node.left);
                if(node.right != null) que.add(node.right);
            }
        }
        return root;
    }
}
```

使用前序遍历：

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if(root == null) return root;
        // 前序遍历 中->左->右
        TreeNode node = root.left;
        root.left = root.right;
        root.right = node;
        invertTree(root.left);
        invertTree(root.right);
        return root;
    }
}
```

# 101. 对称二叉树

## 题目描述

给你一个二叉树的根节点 `root` ， 检查它是否轴对称。

 

**示例 1：**

![img](./LeetCode--代码随想录(二叉树).assets/1698026966-JDYPDU-image.png)

```
输入：root = [1,2,2,3,4,4,3]
输出：true
```

**示例 2：**

![img](./LeetCode--代码随想录(二叉树).assets/1698027008-nPFLbM-image.png)

```
输入：root = [1,2,2,null,3,null,3]
输出：false
```

 

**提示：**

- 树中节点数目在范围 `[1, 1000]` 内
- `-100 <= Node.val <= 100`

 

**进阶：**你可以运用递归和迭代两种方法解决这个问题吗？

## 代码

使用递归法：

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    
    public boolean isSame(TreeNode left, TreeNode right){
        if(left == null || right == null) return left==right;
        // 这一层的左右 && 下一层的外侧 && 下一层的里侧
        return left.val == right.val && isSame(left.left, right.right) && isSame(left.right, right.left);
    }
    
    public boolean isSymmetric(TreeNode root) {
        // 使用递归法
        if(root == null) return true;
        return isSame(root.left, root.right);
    }
}
```

借助列表，层序遍历：

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public boolean isSymmetric(TreeNode root) {
        List<TreeNode> que = new LinkedList<>();
        if(root.left == null && root.right == null) return true;
        que.add(root);
        while(!que.isEmpty()){
            int size = que.size();
            // 先做比较
            for(int i=0; i<size/2; i++){
                TreeNode pre = que.get(i);
                TreeNode last = que.get(size-i-1);
                if(pre == last) continue;
                if(pre == null || last == null || pre.val != last.val) return false;
            }
            // 再新增子节点 null也要
            for(int i=0; i<size; i++){
                TreeNode node = que.remove(0);
                if(node != null){
                    que.add(node.left);
                    que.add(node.right);
                }
                
            }
        }
        return true;
    }
}
```

# 100.相同的树

## 题目描述

给你两棵二叉树的根节点 `p` 和 `q` ，编写一个函数来检验这两棵树是否相同。

如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。

 

**示例 1：**

![img](./LeetCode--代码随想录(二叉树).assets/ex1.jpg)

```
输入：p = [1,2,3], q = [1,2,3]
输出：true
```

**示例 2：**

![img](./LeetCode--代码随想录(二叉树).assets/ex2.jpg)

```
输入：p = [1,2], q = [1,null,2]
输出：false
```

**示例 3：**

![img](./LeetCode--代码随想录(二叉树).assets/ex3.jpg)

```
输入：p = [1,2,1], q = [1,1,2]
输出：false
```

 

**提示：**

- 两棵树上的节点数目都在范围 `[0, 100]` 内
- `-10^4 <= Node.val <= 10^4`

## 代码

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {

    public boolean isSameTree(TreeNode p, TreeNode q) {
        // 必须同时为null
        if(p==null || q==null) return p==q;
        // 逻辑判断 所有条件都要满足
        return p.val==q.val && isSameTree(p.left,q.left) && isSameTree(p.right,q.right);        
    }
}
```

# 572.另一棵树的子树

## 题目描述

给你两棵二叉树 `root` 和 `subRoot` 。检验 `root` 中是否包含和 `subRoot` 具有相同结构和节点值的子树。如果存在，返回 `true` ；否则，返回 `false` 。

二叉树 `tree` 的一棵子树包括 `tree` 的某个节点和这个节点的所有后代节点。`tree` 也可以看做它自身的一棵子树。

 

**示例 1：**

![img](./LeetCode--代码随想录(二叉树).assets/1724998676-cATjhe-image.png)

```
输入：root = [3,4,5,1,2], subRoot = [4,1,2]
输出：true
```

**示例 2：**

![img](./LeetCode--代码随想录(二叉树).assets/1724998698-sEJWnq-image.png)

```
输入：root = [3,4,5,1,2,null,null,null,null,0], subRoot = [4,1,2]
输出：false
```

 

**提示：**

- `root` 树上的节点数量范围是 `[1, 2000]`
- `subRoot` 树上的节点数量范围是 `[1, 1000]`
- `-10^4 <= root.val <= 10^4`
- `-10^4 <= subRoot.val <= 10^4`

## 代码

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {

    public boolean isSameTree(TreeNode root, TreeNode subRoot){
        if(root==null || subRoot==null) return root==subRoot;
        return root.val==subRoot.val && isSameTree(root.left,subRoot.left) && isSameTree(root.right, subRoot.right);
    }

    public boolean isSubtree(TreeNode root, TreeNode subRoot) {
        if(root==null || subRoot==null) return root==subRoot;
        return isSameTree(root, subRoot) || isSubtree(root.left, subRoot) || isSubtree(root.right, subRoot);
    }
}
```

# 222.完全二叉树的节点个数

## 题目描述

给你一棵 **完全二叉树** 的根节点 `root` ，求出该树的节点个数。

[完全二叉树](https://baike.baidu.com/item/完全二叉树/7773232?fr=aladdin) 的定义如下：在完全二叉树中，除了最底层节点可能没填满外，其余每层节点数都达到最大值，并且最下面一层的节点都集中在该层最左边的若干位置。若最底层为第 `h` 层（从第 0 层开始），则该层包含 `1~ 2h` 个节点。

 

**示例 1：**

![img](./LeetCode--代码随想录(二叉树).assets/complete.jpg)

```
输入：root = [1,2,3,4,5,6]
输出：6
```

**示例 2：**

```
输入：root = []
输出：0
```

**示例 3：**

```
输入：root = [1]
输出：1
```

 

**提示：**

- 树中节点的数目范围是`[0, 5 * 10^4]`
- `0 <= Node.val <= 5 * 10^4`
- 题目数据保证输入的树是 **完全二叉树**

 

**进阶：**遍历树来统计节点是一种时间复杂度为 `O(n)` 的简单解决方案。你可以设计一个更快的算法吗？



## 代码

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public int countNodes(TreeNode root) {
        if(root == null) return 0;
        // 计算左右子树高度
        TreeNode left = root.left;
        TreeNode right = root.right;
        int leftHeight = 0;
        int rightHeight = 0;
        while(left != null){
            leftHeight++;
            left = left.left;
        }
        while(right != null){
            rightHeight++;
            right = right.right;
        }
        if(leftHeight == rightHeight){
            // 如果左右子树高度相同 则为满二叉树
            // 节点数为 2^height - 1
            // 注意(2<<1) 相当于2^2，所以leftHeight初始为0
            return (2<<leftHeight) - 1;
        }
        // 如果不是满二叉树 当作普通二叉树递归处理
        return countNodes(root.left) + countNodes(root.right) + 1;
    }
}
```

# 110.平衡二叉树

## 题目描述

给定一个二叉树，判断它是否是 平衡二叉树 （**平衡二叉树** 是指该树所有节点的左右子树的高度相差不超过 1。）

 

**示例 1：**

![img](./LeetCode--代码随想录(二叉树).assets/balance_1.jpg)

```
输入：root = [3,9,20,null,null,15,7]
输出：true
```

**示例 2：**

![img](./LeetCode--代码随想录(二叉树).assets/balance_2.jpg)

```
输入：root = [1,2,2,3,3,null,null,4,4]
输出：false
```

**示例 3：**

```
输入：root = []
输出：true
```

 

**提示：**

- 树中的节点数在范围 `[0, 5000]` 内
- `-10^4 <= Node.val <= 10^4`

## 代码

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {

    public int getHeight(TreeNode node){
        // null结点为高度0
        if(node == null) return 0;

        // 计算左子树高度
        int leftHeight = getHeight(node.left);
        // 传递标志位
        if(leftHeight == -1) return -1;
        // 计算右子树高度
        int rightHeight = getHeight(node.right);
        // 传递标志位
        if(rightHeight == -1) return -1;
        // 如果左右子树高度相差大于1，则返回-1作为标志位
        if(leftHeight - rightHeight > 1 || leftHeight - rightHeight < -1) 
            return -1;
        else
        // 当前节点高度为左右子树中最高高度 + 1
            return Math.max(leftHeight, rightHeight) + 1;

    }

    public boolean isBalanced(TreeNode root) {
        if(getHeight(root) == -1) return false;
        return true;
    }
}
```

#  257. 二叉树的所有路径

## 题目描述

给你一个二叉树的根节点 `root` ，按 **任意顺序** ，返回所有从根节点到叶子节点的路径。

**叶子节点** 是指没有子节点的节点。

 

**示例 1：**

![img](./LeetCode--代码随想录(二叉树).assets/paths-tree.jpg)

```
输入：root = [1,2,3,null,5]
输出：["1->2->5","1->3"]
```

**示例 2：**

```
输入：root = [1]
输出：["1"]
```

 

**提示：**

- 树中节点的数目在范围 `[1, 100]` 内
- `-100 <= Node.val <= 100`



## 代码

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {

    public void traversal(TreeNode node, List<String> result, List<TreeNode> nodes){
        if(node == null) return;
        // 前序遍历 中间节点操作
        nodes.add(node);
        // 终止条件，遍历到叶子结点时
        if(node.left == null && node.right == null){
            // 构造string
            StringBuilder path = new StringBuilder();
            int size = nodes.size();
            for(int i=0; i<size-1; i++){
                path.append(nodes.get(i).val);
                path.append("->");
            }
            path.append(nodes.get(size-1).val);
            // 添加结果列表
            result.add(path.toString());
            return;
        }
        // 左边节点操作
        if(node.left != null){
            traversal(node.left, result, nodes);
            // 回溯弹出添加的左节点
            nodes.remove(nodes.size()-1);
        }
        // 右边节点操作
        if(node.right != null){
            traversal(node.right, result, nodes);
            // 回溯弹出添加的右节点
            nodes.remove(nodes.size()-1);
        }
        return;
    }

    public List<String> binaryTreePaths(TreeNode root) {
        List<String> result = new ArrayList<>();
        // 存放路径节点的队列
        List<TreeNode> nodes = new ArrayList<>();
        traversal(root, result, nodes);
        return result;
    }
}
```

# 404.左叶子之和

## 题目描述

给定二叉树的根节点 `root` ，返回所有左叶子之和。

 

**示例 1：**

![img](./LeetCode--代码随想录(二叉树).assets/leftsum-tree.jpg)

```
输入: root = [3,9,20,null,null,15,7] 
输出: 24 
解释: 在这个二叉树中，有两个左叶子，分别是 9 和 15，所以返回 24
```

**示例 2:**

```
输入: root = [1]
输出: 0
```

 

**提示:**

- 节点数在 `[1, 1000]` 范围内
- `-1000 <= Node.val <= 1000`

## 代码

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {

    public int sumOfLeftLeaves(TreeNode root) {
        if(root == null) return 0;
        int leftValue = sumOfLeftLeaves(root.left);
        int rightValue = sumOfLeftLeaves(root.right);
        // 保证为左叶子结点
        if(root.left != null && root.left.left == null && root.left.right == null) leftValue = root.left.val;
        int sum = leftValue + rightValue;
        return sum;
    }
}
```

# 513.找树左下角的值

## 题目描述

给定一个二叉树的 **根节点** `root`，请找出该二叉树的 **最底层 最左边** 节点的值。

假设二叉树中至少有一个节点。

 

**示例 1:**

![img](./LeetCode--代码随想录(二叉树).assets/tree1-1777729108391-12.jpg)

```
输入: root = [2,1,3]
输出: 1
```

**示例 2:**

![img](./LeetCode--代码随想录(二叉树).assets/tree2.jpg)

```
输入: [1,2,3,4,null,5,6,null,null,7]
输出: 7
```

 

**提示:**

- 二叉树的节点个数的范围是 `[1,10^4]`
- `-231 <= Node.val <= 231 - 1` 



## 代码

### 迭代法

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {

    // 迭代法 层序遍历
    public int findBottomLeftValue(TreeNode root) {
        int result = root.val;
        if(root.left==null && root.right==null) return result;
        // 分层存放
        Queue<TreeNode> que = new ArrayDeque<>();
        que.add(root);
        while(!que.isEmpty()){
            int size = que.size();
            result = que.peek().val;
            for(int i=0; i<size; i++){
                TreeNode node = que.poll();
                if(node.left != null) que.add(node.left);
                if(node.right != null) que.add(node.right);
            }
        }
        return result;
    }
}
```

### 递归法

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {

    public int result;

    public int maxDepth=0;

    public void traverse(TreeNode node, int depth){
        if(node==null) return;
        // 前序遍历 先处理中间节点
        if(depth>maxDepth && node.left == null && node.right ==null){
            maxDepth = depth;
            result = node.val;
            return;
        }
        // 处理左节点
        if(node.left != null){
            depth++;
            traverse(node.left, depth);
            depth--; //回溯
        }
        // 处理右节点
        if(node.right != null){
            depth++;
            traverse(node.right, depth);
            depth--; //回溯
        }
        return;
    }

    public int findBottomLeftValue(TreeNode root) {
        traverse(root, 1);
        return result;
    }
}
```



# 112. 路径总和

## 题目描述

给你二叉树的根节点 `root` 和一个表示目标和的整数 `targetSum` 。判断该树中是否存在 **根节点到叶子节点** 的路径，这条路径上所有节点值相加等于目标和 `targetSum` 。如果存在，返回 `true` ；否则，返回 `false` 。

**叶子节点** 是指没有子节点的节点。

 

**示例 1：**

![img](./LeetCode--代码随想录(二叉树).assets/pathsum1.jpg)

```
输入：root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22
输出：true
解释：等于目标和的根节点到叶节点路径如上图所示。
```

**示例 2：**

![img](./LeetCode--代码随想录(二叉树).assets/pathsum2.jpg)

```
输入：root = [1,2,3], targetSum = 5
输出：false
解释：树中存在两条根节点到叶子节点的路径：
(1 --> 2): 和为 3
(1 --> 3): 和为 4
不存在 sum = 5 的根节点到叶子节点的路径。
```

**示例 3：**

```
输入：root = [], targetSum = 0
输出：false
解释：由于树是空的，所以不存在根节点到叶子节点的路径。
```

 

**提示：**

- 树中节点的数目在范围 `[0, 5000]` 内
- `-1000 <= Node.val <= 1000`
- `-1000 <= targetSum <= 1000`

## 代码

清晰版本：

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {

    public boolean traversal(TreeNode node, int count){
        // 遇到叶子结点，进行处理
        if(node.left == null && node.right == null && count == 0) return true;
        if(node.left == null && node.right == null) return false;
        
        // 处理左节点
        if(node.left != null){
            count -= node.left.val; // 递归，处理结果
            if(traversal(node.left, count)) return true;
            count += node.left.val; // 回溯
        }
        // 处理右节点
        if(node.right != null){
            count -= node.right.val; // 递归，处理结果
            if(traversal(node.right, count)) return true;
            count += node.right.val; // 回溯
        }
        return false;
    }

    public boolean hasPathSum(TreeNode root, int targetSum) {
        if(root == null) return false;
        return traversal(root, targetSum - root.val);
    }
}
```

简洁版本：

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {

    public boolean hasPathSum(TreeNode root, int targetSum) {
        if(root == null) return false;
        // 迭代终止条件
        if(root.left == null && root.right == null) return targetSum == root.val;
        return hasPathSum(root.left, targetSum - root.val) || hasPathSum(root.right, targetSum -root.val);
    }
}
```

# 113.路径总和 II

## 题目描述

给你二叉树的根节点 `root` 和一个整数目标和 `targetSum` ，找出所有 **从根节点到叶子节点** 路径总和等于给定目标和的路径。

**叶子节点** 是指没有子节点的节点。

 

**示例 1：**

![img](./LeetCode--代码随想录(二叉树).assets/pathsumii1.jpg)

```
输入：root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
输出：[[5,4,11,2],[5,8,4,5]]
```

**示例 2：**

![img](./LeetCode--代码随想录(二叉树).assets/pathsum2-1777814505611-6.jpg)

```
输入：root = [1,2,3], targetSum = 5
输出：[]
```

**示例 3：**

```
输入：root = [1,2], targetSum = 0
输出：[]
```

 

**提示：**

- 树中节点总数在范围 `[0, 5000]` 内
- `-1000 <= Node.val <= 1000`
- `-1000 <= targetSum <= 1000`

## 代码

注意要拷贝副本！！！

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {

    public List<List<Integer>> result = new ArrayList<>();

    // 路径
    public List<Integer> path = new ArrayList<>();

    public void traverse(TreeNode node, int count){
        // 处理叶子结点
        if(node.left == null && node.right == null && count == 0){
            /************************************************************** 
            在 result.add(path) 时，直接把同一个 path 对象添加到了结果列表中。
            后续回溯操作（path.remove(...)）会修改这个唯一的列表对象，
            导致 result 里存的所有引用最终都指向了同一个被清空、只剩最后一步元素的列表。
            错误写法: 
            result.add(path);
            *********************************************************************/

            // 正确写法
            result.add(new ArrayList<>(path));
            return;
        }
        if(node.left == null && node.right == null) return;
        // 处理左节点
        if(node.left != null){
            // 将当前节点加入路径
            path.add(node.left.val);
            traverse(node.left, count - node.left.val);
            path.remove(path.size()-1);
        }
        // 处理右节点
        if(node.right != null){
            // 将当前节点加入路径
            path.add(node.right.val);
            traverse(node.right, count - node.right.val);
            path.remove(path.size()-1);
        }
        return;
    }

    public List<List<Integer>> pathSum(TreeNode root, int targetSum) {
        if(root==null) return result;
        path.add(root.val);
        traverse(root, targetSum - root.val);
        return result;
    }
}
```

# 106.从中序与后序遍历序列构造二叉树

## 题目描述

给定两个整数数组 `inorder` 和 `postorder` ，其中 `inorder` 是二叉树的中序遍历， `postorder` 是同一棵树的后序遍历，请你构造并返回这颗 *二叉树* 。

 

**示例 1:**

![img](./LeetCode--代码随想录(二叉树).assets/tree.jpg)

```
输入：inorder = [9,3,15,20,7], postorder = [9,15,7,20,3]
输出：[3,9,20,null,null,15,7]
```

**示例 2:**

```
输入：inorder = [-1], postorder = [-1]
输出：[-1]
```

 

**提示:**

- `1 <= inorder.length <= 3000`
- `postorder.length == inorder.length`
- `-3000 <= inorder[i], postorder[i] <= 3000`
- `inorder` 和 `postorder` 都由 **不同** 的值组成
- `postorder` 中每一个值都在 `inorder` 中
- `inorder` **保证**是树的中序遍历
- `postorder` **保证**是树的后序遍历

## 解题思路

![image-20260503212528085](./LeetCode--代码随想录(二叉树).assets/image-20260503212528085.png)

![image-20260503212720189](./LeetCode--代码随想录(二叉树).assets/image-20260503212720189.png) 

## 代码

### 构造新数组

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        if(inorder == null) return null;
        // 在后序数组中找到最后一个元素
        int rootVal = postorder[postorder.length - 1];
        
        TreeNode root = new TreeNode(rootVal);
        // 已经是叶子结点
        if(postorder.length == 1) return root;
        // 在中序遍历中找到根节点下标
        int indexOfRoot = 0;
        for(int i=0; i<inorder.length; i++){
            if(inorder[i] == rootVal){
                indexOfRoot = i;
                break;
            }
        }
        // 切割中序遍历的左右子树
        int[] leftInOrder = new int[indexOfRoot];
        // 构造左子树中序遍历数组
        if(indexOfRoot != 0){
            for(int i=0; i<indexOfRoot; i++){
                leftInOrder[i] = inorder[i];
            }
        }else{
            leftInOrder = null;        
        } 
        // 构造右子树中序遍历数组
        int rightSize = inorder.length - indexOfRoot - 1;
        int[] rightInOrder = new int[rightSize];
        if(rightSize != 0){
            for(int i=0; i<rightSize; i++){
                rightInOrder[i] = inorder[i+indexOfRoot+1];
            }
        }else{
            rightInOrder = null;        
        }  
    
        // 切割后序遍历的左右子树
        int[] leftPostOrder = new int[indexOfRoot];
        // 构造左子树后序遍历数组
        if(indexOfRoot != 0){
            for(int i=0; i<indexOfRoot; i++){
                leftPostOrder[i] = postorder[i];
            }
        }else{
            leftPostOrder = null;        
        } 
        int[] rightPostOrder = new int[rightSize];
        // 构造右子树后序遍历数组
        if(rightSize != 0){
            for(int i=0; i<rightSize; i++){
                rightPostOrder[i] = postorder[i+indexOfRoot];
            }
        }else{
            rightPostOrder = null;        
        }  
        TreeNode leftChild = buildTree(leftInOrder, leftPostOrder);
        TreeNode rightChild = buildTree(rightInOrder, rightPostOrder);
        root.left = leftChild;
        root.right = rightChild;
        return root;
    }
}
```

### 记录下标法

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {

    public TreeNode buildHelper(int[] inorder, int inorderStart, int inorderEnd, int[] postorder, int postorderStart, int postorderEnd){
        if(inorderEnd < inorderStart || postorderEnd < postorderStart) return null;
        // 在后序数组中找到最后一个元素
        int rootVal = postorder[postorderEnd];
        
        TreeNode root = new TreeNode(rootVal);
        // 已经是叶子结点
        if(postorderEnd == 0) return root;
        // 在中序遍历中找到根节点下标
        int indexOfRoot = inorderStart;
        for(int i = inorderStart; i <= inorderEnd; i++){
            if(inorder[i] == rootVal){
                indexOfRoot = i;
                break;
            }
        }
        // 切割中序遍历的左右子树
        // 构造左子树中序遍历数组
        int leftInOrderStart = inorderStart;
        int leftInOrderEnd = indexOfRoot - 1;
        // 构造右子树中序遍历数组
        int rightInOrderStart = indexOfRoot + 1;
        int rightInOrderEnd = inorderEnd;

        // 切割后序遍历的左右子树
        int leftSize = indexOfRoot - inorderStart;
        // 构造左子树后序遍历数组
        int leftPostOrderStart = postorderStart;
        int leftPostOrderEnd = postorderStart + leftSize - 1;
        // 构造右子树后序遍历数组
        int rightPostOrderStart = postorderStart + leftSize;
        int rightPostOrderEnd = postorderEnd - 1;
        TreeNode leftChild = buildHelper(inorder, leftInOrderStart, leftInOrderEnd, postorder, leftPostOrderStart, leftPostOrderEnd);
        TreeNode rightChild = buildHelper(inorder, rightInOrderStart, rightInOrderEnd, postorder, rightPostOrderStart, rightPostOrderEnd);
        root.left = leftChild;
        root.right = rightChild;
        return root;
    }

    public TreeNode buildTree(int[] inorder, int[] postorder) {
        if(inorder == null) return null;
        return buildHelper(inorder, 0, inorder.length - 1, postorder, 0, postorder.length - 1);
    }
}
```

### Map记录中序遍历

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {

    Map<Integer, Integer> map = new HashMap<>();

    public TreeNode buildTree(int[] inorder, int[] postorder) {
        if(inorder.length == 0) return null;
        // 因为每次都需要从先序遍历中找到中间节点进行切分 做成map
        for(int i=0; i<inorder.length; i++){
            // 值为key 下标为value
            map.put(inorder[i],i);
        }
        return buildHelper(inorder, 0, inorder.length - 1, postorder, 0, postorder.length - 1);
    }

    // 左闭右闭区间
    public TreeNode buildHelper(int[] inorder, int inorderStart, int inorderEnd, int[] postorder, int postorderStart, int postorderEnd){
        // 终止条件 数组越界
        if(inorderEnd < inorderStart || postorderEnd < postorderStart) return null;
        // 在后序数组中找到最后一个元素
        int rootVal = postorder[postorderEnd];
        TreeNode root = new TreeNode(rootVal);
        // 已经是叶子结点
        if(postorderEnd == 0) return root;
        // 在中序遍历中找到根节点下标
        int indexOfRoot = map.get(rootVal);

        int leftSize = indexOfRoot - inorderStart;

        TreeNode leftChild = buildHelper(inorder, inorderStart, indexOfRoot - 1, postorder, postorderStart, postorderStart + leftSize - 1);
        TreeNode rightChild = buildHelper(inorder, indexOfRoot + 1, inorderEnd, postorder, postorderStart + leftSize, postorderEnd - 1);
        root.left = leftChild;
        root.right = rightChild;
        return root;
    }
}
```

# 105.从前序和中序遍历序列构造二叉树

## 题目描述

给定两个整数数组 `preorder` 和 `inorder` ，其中 `preorder` 是二叉树的**先序遍历**， `inorder` 是同一棵树的**中序遍历**，请构造二叉树并返回其根节点。

 

**示例 1:**

![img](./LeetCode--代码随想录(二叉树).assets/tree-1777814944893-12.jpg)

```
输入: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
输出: [3,9,20,null,null,15,7]
```

**示例 2:**

```
输入: preorder = [-1], inorder = [-1]
输出: [-1]
```

 

**提示:**

- `1 <= preorder.length <= 3000`
- `inorder.length == preorder.length`
- `-3000 <= preorder[i], inorder[i] <= 3000`
- `preorder` 和 `inorder` 均 **无重复** 元素
- `inorder` 均出现在 `preorder`
- `preorder` **保证** 为二叉树的前序遍历序列
- `inorder` **保证** 为二叉树的中序遍历序列

## 代码

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {

    Map<Integer, Integer> map = new HashMap<>();

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if(preorder.length == 0) return null;
        for(int i=0; i<inorder.length; i++){
            map.put(inorder[i],i);
        }
        return buildHepler(preorder, 0, preorder.length - 1, inorder, 0, inorder.length - 1);
    }

    public TreeNode buildHepler(int[] preorder, int preStart, int preEnd, int[] inorder, int inStart, int inEnd){
        // 终止递归条件
        if(preStart > preEnd || inStart > inEnd) return null;
        // 取出先序遍历第一个 在中序遍历中找到他的位置
        TreeNode root = new TreeNode(preorder[preStart]);
        int index = map.get(preorder[preStart]);
        // 记录左边长度
        int leftLength = index - inStart;

        root.left = buildHepler(preorder, preStart + 1, preStart + leftLength, inorder, inStart, inStart + leftLength);
        root.right = buildHepler(preorder, preStart + 1 + leftLength, preEnd, inorder, index + 1, inEnd);
        return root;
    }

}
```

# 654.最大二叉树

## 题目描述

给定一个不重复的整数数组 `nums` 。 **最大二叉树** 可以用下面的算法从 `nums` 递归地构建:

1. 创建一个根节点，其值为 `nums` 中的最大值。
2. 递归地在最大值 **左边** 的 **子数组前缀上** 构建左子树。
3. 递归地在最大值 **右边** 的 **子数组后缀上** 构建右子树。

返回 *`nums` 构建的* ***最大二叉树\*** 。

 

**示例 1：**

![img](./LeetCode--代码随想录(二叉树).assets/tree1-1777815033224-15.jpg)

```
输入：nums = [3,2,1,6,0,5]
输出：[6,3,5,null,2,0,null,null,1]
解释：递归调用如下所示：
- [3,2,1,6,0,5] 中的最大值是 6 ，左边部分是 [3,2,1] ，右边部分是 [0,5] 。
    - [3,2,1] 中的最大值是 3 ，左边部分是 [] ，右边部分是 [2,1] 。
        - 空数组，无子节点。
        - [2,1] 中的最大值是 2 ，左边部分是 [] ，右边部分是 [1] 。
            - 空数组，无子节点。
            - 只有一个元素，所以子节点是一个值为 1 的节点。
    - [0,5] 中的最大值是 5 ，左边部分是 [0] ，右边部分是 [] 。
        - 只有一个元素，所以子节点是一个值为 0 的节点。
        - 空数组，无子节点。
```

**示例 2：**

![img](./LeetCode--代码随想录(二叉树).assets/tree2-1777815033225-17.jpg)

```
输入：nums = [3,2,1]
输出：[3,null,2,null,1]
```

 

**提示：**

- `1 <= nums.length <= 1000`
- `0 <= nums[i] <= 1000`
- `nums` 中的所有整数 **互不相同**

## 图解思路

![image-20260503213141482](./LeetCode--代码随想录(二叉树).assets/image-20260503213141482.png)

## 代码

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {

    // 传入左闭右闭区间
    public TreeNode traversal(int[] nums, int start, int end){
        // 递归终止条件
        if(start > end) return null;

        // 找出最大值
        int maxValue = nums[start];
        int index = start;
        for(int i=start; i<=end; i++){
            if(nums[i] > maxValue){
                maxValue = nums[i];
                index = i;
            }
        }
        // 构造根节点
        TreeNode root = new TreeNode(maxValue);

        // 递归左子树
        TreeNode leftChild = traversal(nums, start, index - 1);
        // 递归右子树
        TreeNode rightChild = traversal(nums, index + 1, end);
        root.left = leftChild;
        root.right = rightChild;
        return root;
    }

    public TreeNode constructMaximumBinaryTree(int[] nums) {
        return traversal(nums, 0, nums.length - 1);
    }
}
```

# 617.合并二叉树

## 题目描述

给你两棵二叉树： `root1` 和 `root2` 。

想象一下，当你将其中一棵覆盖到另一棵之上时，两棵树上的一些节点将会重叠（而另一些不会）。你需要将这两棵树合并成一棵新二叉树。合并的规则是：如果两个节点重叠，那么将这两个节点的值相加作为合并后节点的新值；否则，**不为** null 的节点将直接作为新二叉树的节点。

返回合并后的二叉树。

**注意:** 合并过程必须从两个树的根节点开始。

 

**示例 1：**

![img](./LeetCode--代码随想录(二叉树).assets/merge.jpg)

```
输入：root1 = [1,3,2,5], root2 = [2,1,3,null,4,null,7]
输出：[3,4,5,5,4,null,7]
```

**示例 2：**

```
输入：root1 = [1], root2 = [1,2]
输出：[2,2]
```

 

**提示：**

- 两棵树中的节点数目在范围 `[0, 2000]` 内
- `-10^4 <= Node.val <= 10^4`

## 代码

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        // 确定遍历终止条件
        if(root1 == null) return root2;
        if(root2 == null) return root1;

        // 前序遍历 处理中间节点
        // 借用root1节点作为返回节点
        root1.val = root1.val + root2.val;
        if(root1.left != null || root2.left != null) root1.left = mergeTrees(root1.left, root2.left);
        if(root1.right != null || root2.right != null) root1.right = mergeTrees(root1.right, root2.right);

        return root1;
        
    }
}
```

# 700.二叉搜索树中的搜索

## 题目描述

给定二叉搜索树（BST）的根节点 `root` 和一个整数值 `val`。

你需要在 BST 中找到节点值等于 `val` 的节点。 返回以该节点为根的子树。 如果节点不存在，则返回 `null` 。

 

**示例 1:**

![img](./LeetCode--代码随想录(二叉树).assets/tree1-1777987752773-1.jpg)

```
输入：root = [4,2,7,1,3], val = 2
输出：[2,1,3]
```

**示例 2:**

![img](./LeetCode--代码随想录(二叉树).assets/tree2-1777987752773-3.jpg)

```
输入：root = [4,2,7,1,3], val = 5
输出：[]
```

 

**提示：**

- 树中节点数在 `[1, 5000]` 范围内
- `1 <= Node.val <= 107`
- `root` 是二叉搜索树
- `1 <= val <= 107`

## 代码

### 迭代法

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
        // 迭代法
        while(root != null){
            // 二叉搜索树，左孩子的值小于根节点，右孩子的值大于根节点
            if(root.val > val) root = root.left;
            else if(root.val < val) root = root.right;
            else return root;
        }
        return null;
        
    }
}
```

### 递归法

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
        // 递归法
        if(root == null || root.val == val) return root;

        // 二叉搜索树，左孩子的值小于根节点，右孩子的值大于根节点
        if(root.val > val) return searchBST(root.left, val);
        if(root.val < val) return searchBST(root.right, val);
        return null;
    }
}
```

# 98.验证二叉搜索树

## 题目描述

给你一个二叉树的根节点 `root` ，判断其是否是一个有效的二叉搜索树。

**有效** 二叉搜索树定义如下：

- 节点的左子树只包含 **严格小于** 当前节点的数。
- 节点的右子树只包含 **严格大于** 当前节点的数。
- 所有左子树和右子树自身必须也是二叉搜索树。

 

**示例 1：**

![img](./LeetCode--代码随想录(二叉树).assets/tree1-1777987837733-7.jpg)

```
输入：root = [2,1,3]
输出：true
```

**示例 2：**

![img](./LeetCode--代码随想录(二叉树).assets/tree2-1777987837734-8.jpg)

```
输入：root = [5,1,4,null,null,3,6]
输出：false
解释：根节点的值是 5 ，但是右子节点的值是 4 。
```

 

**提示：**

- 树中节点数目范围在`[1, 10^4]` 内
- `-231 <= Node.val <= 231 - 1`

## 图解思路

中序遍历体现二叉搜索树的特征：

![image-20260505213223706](./LeetCode--代码随想录(二叉树).assets/image-20260505213223706.png)

![image-20260505213228559](./LeetCode--代码随想录(二叉树).assets/image-20260505213228559.png)

## 代码

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {

    public TreeNode pre = null;

    public boolean isValidBST(TreeNode root) {

        if(root == null) return false;

        // 遍历左子树
        boolean isLeftValid = true;
        if(root.left != null) isLeftValid = isValidBST(root.left);

        // 处理中间节点
        boolean valid = true;
        if(pre != null && root.val <= pre.val){
            valid = false;
        }
        pre = root;

        // 遍历右子树
        boolean isRightValid = true;
        if(root.right != null) isRightValid = isValidBST(root.right);

        return isLeftValid && isRightValid && valid;
    }
}
```

# 530.二叉搜索树的最小绝对差

## 题目描述

给你一个二叉搜索树的根节点 `root` ，返回 **树中任意两不同节点值之间的最小差值** 。

差值是一个正数，其数值等于两值之差的绝对值。

 

**示例 1：**

![img](./LeetCode--代码随想录(二叉树).assets/bst1.jpg)

```
输入：root = [4,2,6,1,3]
输出：1
```

**示例 2：**

![img](./LeetCode--代码随想录(二叉树).assets/bst2.jpg)

```
输入：root = [1,0,48,null,null,12,49]
输出：1
```

 

**提示：**

- 树中节点的数目范围是 `[2, 10^4]`
- `0 <= Node.val <= 105`

## 代码

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {

    TreeNode pre = null;

    int min = Integer.MAX_VALUE;

    public int getMinimumDifference(TreeNode root) {
        // 采用中序遍历
        if(root.left != null) getMinimumDifference(root.left);

        // 处理中间节点
        if(pre != null && root.val - pre.val < min){
            min = root.val - pre.val;
        }
        pre = root;

        // 遍历右子树
        if(root.right != null) getMinimumDifference(root.right);

        return min;
    }
}
```

# 501.二叉搜索树中的众数

## 题目描述

给你一个含重复值的二叉搜索树（BST）的根节点 `root` ，找出并返回 BST 中的所有 [众数](https://baike.baidu.com/item/众数/44796)（即，出现频率最高的元素）。

如果树中有不止一个众数，可以按 **任意顺序** 返回。

假定 BST 满足如下定义：

- 结点左子树中所含节点的值 **小于等于** 当前节点的值
- 结点右子树中所含节点的值 **大于等于** 当前节点的值
- 左子树和右子树都是二叉搜索树

 

**示例 1：**

![img](./LeetCode--代码随想录(二叉树).assets/mode-tree.jpg)

```
输入：root = [1,null,2,2]
输出：[2]
```

**示例 2：**

```
输入：root = [0]
输出：[0]
```

 

**提示：**

- 树中节点的数目在范围 `[1, 10^4]` 内
- `-105 <= Node.val <= 105`

 

**进阶：**你可以不使用额外的空间吗？（假设由递归产生的隐式调用栈的开销不被计算在内）



## 代码

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {

    public List<Integer> resultList = new ArrayList<>();

    public TreeNode pre = null;

    public int count = 0;

    public int maxCount = 0;

    public void traversal(TreeNode cur){
        // 中序遍历二叉搜索树
        if(cur == null) return;

        // 遍历左子树
        traversal(cur.left);
        // 处理中间节点
        // 假如不是第一次遇到该值
        if(pre != null && cur.val == pre.val){
            count++;
        } else{
            // 第一次遇到新值
            count = 1;
        }
        pre = cur;
        // 处理结果集
        if(count > maxCount){
            maxCount = count;
            resultList.clear();
            resultList.add(cur.val);
        } else if(count == maxCount){
            resultList.add(cur.val);
        }

        // 遍历右子树
        traversal(cur.right);
        return;

    }


    public int[] findMode(TreeNode root) {
        traversal(root);
        int[] res = new int[resultList.size()];
        for(int i=0; i<resultList.size(); i++){
            res[i] = resultList.get(i);
        }
        return res;
        // return resultList.stream().mapToInt(Integer::intValue).toArray();
    }
}
```

# 236. 二叉树的最近公共祖先

## 题目描述

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

[百度百科](https://baike.baidu.com/item/最近公共祖先/8918834?fr=aladdin)中最近公共祖先的定义为：“对于有根树 T 的两个节点 p、q，最近公共祖先表示为一个节点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（**一个节点也可以是它自己的祖先**）。”

 

**示例 1：**

![img](./LeetCode--代码随想录(二叉树).assets/binarytree.png)

```
输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出：3
解释：节点 5 和节点 1 的最近公共祖先是节点 3 。
```

**示例 2：**

![img](./LeetCode--代码随想录(二叉树).assets/binarytree.png)

```
输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出：5
解释：节点 5 和节点 4 的最近公共祖先是节点 5 。因为根据定义最近公共祖先节点可以为节点本身。
```

**示例 3：**

```
输入：root = [1,2], p = 1, q = 2
输出：1
```

 

**提示：**

- 树中节点数目在范围 `[2, 105]` 内。
- `-109 <= Node.val <= 109`
- 所有 `Node.val` `互不相同` 。
- `p != q`
- `p` 和 `q` 均存在于给定的二叉树中。

## 代码

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {

    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {

        // 递归终止条件：
        // 如果当前节点为空，直接返回 null
        if(root == null) return null;

        // 后序遍历：先递归查找左子树
        TreeNode leftResult = lowestCommonAncestor(root.left, p, q);

        // 后序遍历：再递归查找右子树
        TreeNode rightResult = lowestCommonAncestor(root.right, p, q);

        // 如果当前节点就是 p 或 q
        // 说明已经找到其中一个目标节点
        if(root.val == p.val || root.val == q.val) {
            return root;
        }

        // 情况1：
        // 左子树找到了目标节点，右子树没找到
        // 说明公共祖先在左子树
        else if(leftResult != null && rightResult == null) {
            return leftResult;
        }

        // 情况2：
        // 右子树找到了目标节点，左子树没找到
        // 说明公共祖先在右子树
        else if(leftResult == null && rightResult != null) {
            return rightResult;
        }

        // 情况3：
        // 左右子树都找到了目标节点
        // 说明当前 root 就是最近公共祖先
        else if(leftResult != null && rightResult != null) {
            return root;
        }

        // 情况4：
        // 左右子树都没找到
        return null;
    }
}
```

# 235. 二叉搜索树的最近公共祖先

## 题目描述

给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。

[百度百科](https://baike.baidu.com/item/最近公共祖先/8918834?fr=aladdin)中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（**一个节点也可以是它自己的祖先**）。”

例如，给定如下二叉搜索树: root = [6,2,8,0,4,7,9,null,null,3,5]

![img](./LeetCode--代码随想录(二叉树).assets/binarysearchtree_improved.png)

 

**示例 1:**

```
输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
输出: 6 
解释: 节点 2 和节点 8 的最近公共祖先是 6。
```

**示例 2:**

```
输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
输出: 2
解释: 节点 2 和节点 4 的最近公共祖先是 2, 因为根据定义最近公共祖先节点可以为节点本身。
```

 

**说明:**

- 所有节点的值都是唯一的。
- p、q 为不同节点且均存在于给定的二叉搜索树中。

## 图解思路

![image-20260509202050024](./LeetCode--代码随想录(二叉树).assets/image-20260509202050024.png)

![image-20260509202057814](./LeetCode--代码随想录(二叉树).assets/image-20260509202057814.png)

## 代码

### 迭代法

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */

class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // 迭代法
        while(root != null){
            if(root.val > p.val && root.val > q.val){
                root = root.left;
            } else if(root.val < p.val && root.val < q.val){
                root = root.right;
            } else{
                return root;
            }
        }
        return root;
    }
}
```

### 递归法

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */

class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null) return root;
        if(root.val > p.val && root.val > q.val){
            return lowestCommonAncestor(root.left, p, q);
        }
        if(root.val < p.val && root.val < q.val){
            return lowestCommonAncestor(root.right, p, q);
        }
        return root;
    }
}
```

# 701.二叉搜索树中的插入操作

## 题目描述

给定二叉搜索树（BST）的根节点 `root` 和要插入树中的值 `value` ，将值插入二叉搜索树。 返回插入后二叉搜索树的根节点。 输入数据 **保证** ，新值和原始二叉搜索树中的任意节点值都不同。

**注意**，可能存在多种有效的插入方式，只要树在插入后仍保持为二叉搜索树即可。 你可以返回 **任意有效的结果** 。

 

**示例 1：**

![img](./LeetCode--代码随想录(二叉树).assets/insertbst.jpg)

```
输入：root = [4,2,7,1,3], val = 5
输出：[4,2,7,1,3,5]
解释：另一个满足题目要求可以通过的树是：
```

**示例 2：**

```
输入：root = [40,20,60,10,30,50,70], val = 25
输出：[40,20,60,10,30,50,70,null,null,25]
```

**示例 3：**

```
输入：root = [4,2,7,1,3,null,null,null,null,null,null], val = 5
输出：[4,2,7,1,3,5]
```

 

**提示：**

- 树中的节点数将在 `[0, 10^4]`的范围内。
- `-108 <= Node.val <= 108`
- 所有值 `Node.val` 是 **独一无二** 的。
- `-108 <= val <= 108`
- **保证** `val` 在原始BST中不存在。

## 代码

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {

    /**
     * 向二叉搜索树插入一个新节点
     *
     * @param root 树的根节点
     * @param val  要插入的值
     * @return 插入后的树的根节点
     */
    public TreeNode insertIntoBST(TreeNode root, int val) {
        // 如果树为空，直接返回新节点作为根节点
        if (root == null) return new TreeNode(val);

        TreeNode cur = root;     // 当前遍历节点
        TreeNode parent = null;  // 父节点，用于最后插入

        // 遍历找到合适的插入位置
        while (cur != null) {
            parent = cur;
            if (val < cur.val) {
                cur = cur.left;   // 插入值小于当前节点，往左子树走
            } else { // val > cur.val
                cur = cur.right;  // 插入值大于当前节点，往右子树走
            }
        }

        // 根据插入值与父节点比较，确定插入左还是右
        if (val < parent.val) {
            parent.left = new TreeNode(val);
        } else {
            parent.right = new TreeNode(val);
        }

        return root; // 返回树的根节点
    }
}
```

# 450.删除二叉搜索树中的节点

## 题目描述

给定一个二叉搜索树的根节点 **root** 和一个值 **key**，删除二叉搜索树中的 **key** 对应的节点，并保证二叉搜索树的性质不变。返回二叉搜索树（有可能被更新）的根节点的引用。

一般来说，删除节点可分为两个步骤：

1. 首先找到需要删除的节点；
2. 如果找到了，删除它。

 

**示例 1:**

![img](./LeetCode--代码随想录(二叉树).assets/del_node_1.jpg)

```
输入：root = [5,3,6,2,4,null,7], key = 3
输出：[5,4,6,2,null,null,7]
解释：给定需要删除的节点值是 3，所以我们首先找到 3 这个节点，然后删除它。
一个正确的答案是 [5,4,6,2,null,null,7], 如下图所示。
另一个正确答案是 [5,2,6,null,4,null,7]。
```

**示例 2:**

```
输入: root = [5,3,6,2,4,null,7], key = 0
输出: [5,3,6,2,4,null,7]
解释: 二叉树不包含值为 0 的节点
```

**示例 3:**

```
输入: root = [], key = 0
输出: []
```

 

**提示:**

- 节点数的范围 `[0, 10^4]`.
- `-105 <= Node.val <= 105`
- 节点值唯一
- `root` 是合法的二叉搜索树
- `-105 <= key <= 105`

 

**进阶：** 要求算法时间复杂度为 O(h)，h 为树的高度。



## 图解思路

![image-20260510162332802](./LeetCode--代码随想录(二叉树).assets/image-20260510162332802.png)

![image-20260510162340710](./LeetCode--代码随想录(二叉树).assets/image-20260510162340710.png)

## 代码

```java
/**
 * 删除 BST 中值为 key 的节点
 */
class Solution {

    public TreeNode deleteNode(TreeNode root, int key) {

        // 没找到目标节点
        if (root == null) return null;

        // 去左子树删除
        if (key < root.val) {
            root.left = deleteNode(root.left, key);
        }

        // 去右子树删除
        else if (key > root.val) {
            root.right = deleteNode(root.right, key);
        }

        // 找到目标节点
        else {

            // 情况1：
            // 左子树为空，直接返回右子树
            if (root.left == null) {
                return root.right;
            }

            // 情况2：
            // 右子树为空，直接返回左子树
            if (root.right == null) {
                return root.left;
            }

            // 叶子结点包含在情况1/2中
            // 情况3：
            // 左右子树都不为空
            //
            // 用左子树顶替当前节点，
            // 再把右子树接到左子树的最右节点后面
            TreeNode cur = root.left;

            // 找左子树中的最大节点
            while (cur.right != null) {
                cur = cur.right;
            }

            // 接上原来的右子树
            cur.right = root.right;

            return root.left;
        }

        return root;
    }
}
```

#  669. 修剪二叉搜索树

## 题目描述

给你二叉搜索树的根节点 `root` ，同时给定最小边界`low` 和最大边界 `high`。通过修剪二叉搜索树，使得所有节点的值在`[low, high]`中。修剪树 **不应该** 改变保留在树中的元素的相对结构 (即，如果没有被移除，原有的父代子代关系都应当保留)。 可以证明，存在 **唯一的答案** 。

所以结果应当返回修剪好的二叉搜索树的新的根节点。注意，根节点可能会根据给定的边界发生改变。

 

**示例 1：**

![img](./LeetCode--代码随想录(二叉树).assets/trim1.jpg)

```
输入：root = [1,0,2], low = 1, high = 2
输出：[1,null,2]
```

**示例 2：**

![img](./LeetCode--代码随想录(二叉树).assets/trim2.jpg)

```
输入：root = [3,0,4,null,2,null,null,1], low = 1, high = 3
输出：[3,2,null,1]
```

 

**提示：**

- 树中节点数在范围 `[1, 10^4]` 内
- `0 <= Node.val <= 10^4`
- 树中每个节点的值都是 **唯一** 的
- 题目数据保证输入是一棵有效的二叉搜索树
- `0 <= low <= high <= 10^4`

## 图解思路

![image-20260510162637849](./LeetCode--代码随想录(二叉树).assets/image-20260510162637849.png)

## 代码

```java
/**
 * 修剪二叉搜索树
 *
 * 保证所有节点值都在 [low, high] 区间内
 */
class Solution {

    public TreeNode trimBST(TreeNode root, int low, int high) {

        // 空节点直接返回
        if (root == null) return null;

        // 当前节点小于区间下界
        //
        // 由于 BST 左子树所有节点都更小，
        // 因此左子树一定全部无效，可以直接剪掉
        //
        // 只需要处理右子树
        if (root.val < low) {
            return trimBST(root.right, low, high);
        }

        // 当前节点大于区间上界
        //
        // 由于 BST 右子树所有节点都更大，
        // 因此右子树一定全部无效，可以直接剪掉
        //
        // 只需要处理左子树
        if (root.val > high) {
            return trimBST(root.left, low, high);
        }

        // 当前节点合法
        //
        // 递归修剪左右子树，
        // 并重新连接修剪后的结果
        root.left = trimBST(root.left, low, high);
        root.right = trimBST(root.right, low, high);

        return root;
    }
}
```

# 108.将有序数组转换为二叉搜索树

## 题目描述

给你一个整数数组 `nums` ，其中元素已经按 **升序** 排列，请你将其转换为一棵 平衡 二叉搜索树。

 

**示例 1：**

![img](./LeetCode--代码随想录(二叉树).assets/btree1.jpg)

```
输入：nums = [-10,-3,0,5,9]
输出：[0,-3,9,-10,null,5]
解释：[0,-10,5,null,-3,null,9] 也将被视为正确答案：
```

**示例 2：**

![img](./LeetCode--代码随想录(二叉树).assets/btree.jpg)

```
输入：nums = [1,3]
输出：[3,1]
解释：[1,null,3] 和 [3,1] 都是高度平衡二叉搜索树。
```

 

**提示：**

- `1 <= nums.length <= 10^4`
- `-10^4 <= nums[i] <= 10^4`
- `nums` 按 **严格递增** 顺序排列

## 图解思路

![image-20260510162814652](./LeetCode--代码随想录(二叉树).assets/image-20260510162814652.png)

## 代码

```java
/**
 * 将有序数组转换为高度平衡二叉搜索树
 */
class Solution {

    /**
     * 在 nums[left, right] 区间内构造平衡 BST
     *
     * 使用左闭右闭区间：
     * [left, right]
     */
    public TreeNode buildBBST(int[] nums, int left, int right) {

        // 区间为空，无法构造节点
        if (left > right) return null;

        // 取区间中点作为根节点
        //
        // 因为数组本身有序：
        // 中点左边都比它小
        // 中点右边都比它大
        //
        // 因此天然满足 BST 性质
        //
        // 同时中点划分能保证左右子树节点数量尽量接近，
        // 从而保证树高度平衡
        int mid = left + (right - left) / 2;

        TreeNode root = new TreeNode(nums[mid]);

        // 构建左子树
        //
        // 左区间所有元素 < nums[mid]
        root.left = buildBBST(nums, left, mid - 1);

        // 构建右子树
        //
        // 右区间所有元素 > nums[mid]
        root.right = buildBBST(nums, mid + 1, right);

        return root;
    }

    public TreeNode sortedArrayToBST(int[] nums) {

        // 从整个数组开始构建 BST
        return buildBBST(nums, 0, nums.length - 1);
    }
}
```

#  538.把二叉搜索树转换为累加树

## 题目描述

给出二叉 **搜索** 树的根节点 `root`，该树的节点值各不相同，请你将其转换为累加树（Greater Sum Tree），将其转换为一个更大的树，使得原始二叉搜索树中的每个节点值都变为原本值加上原本二叉搜索树中所有比该节点值大的节点值的总和。

提醒一下，二叉搜索树满足下列约束条件：

- 节点的左子树仅包含键 **小于** 节点键的节点。
- 节点的右子树仅包含键 **大于** 节点键的节点。
- 左右子树也必须是二叉搜索树。

**注意：**本题和 1038: https://leetcode.cn/problems/binary-search-tree-to-greater-sum-tree/ 相同

 

**示例 1：**

**![img](./LeetCode--代码随想录(二叉树).assets/tree.png)**

```
输入：[4,1,6,0,2,5,7,null,null,null,3,null,null,null,8]
输出：[30,36,21,36,35,26,15,null,null,null,33,null,null,null,8]
```

**示例 2：**

```
输入：root = [0,null,1]
输出：[1,null,1]
```

**示例 3：**

```
输入：root = [1,0,2]
输出：[3,3,2]
```

**示例 4：**

```
输入：root = [3,2,4,1]
输出：[7,9,4,10]
```

 

**提示：**

- 树中的节点数介于 `0` 和 `10^4` 之间。
- 每个节点的值介于 `-10^4` 和 `10^4` 之间。
- 树中的所有值 **互不相同** 。
- 给定的树为二叉搜索树。

## 代码

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {

    /**
     * pre 始终指向：
     * 当前遍历节点的“前一个节点”
     *
     * 由于采用右中左遍历：
     * pre 保存的其实是：
     * “比当前节点大的最近节点”
     *
     * 并且 pre.val 已经是累加后的值，
     * 因此：
     *
     * 当前节点值 += 所有比它大的节点值
     */
    TreeNode pre = null;

    public TreeNode convertBST(TreeNode root) {

        // 空节点直接返回
        if (root == null) return null;

        // 先遍历右子树
        //
        // BST 右边节点更大，
        // 右中左遍历可以得到从大到小的顺序
        root.right = convertBST(root.right);

        // pre 保存的是：
        // 已经遍历过的、更大的节点
        //
        // 将这些更大的值累加到当前节点
        if (pre != null) {
            root.val += pre.val;
        }

        // 更新 pre
        //
        // 当前节点已经变成累加节点，
        // 后续左子树节点会继续使用它
        pre = root;

        // 最后处理左子树
        root.left = convertBST(root.left);

        return root;
    }
}
```

