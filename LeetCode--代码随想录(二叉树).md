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



- 树中节点数量在 `[1, 104]` 范围内
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
- 树的节点总数在 `[0, 104]` 之间

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

- 二叉树的节点个数的范围是 `[0,104]`
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

- 树中节点的数量在 `[0, 104]` 区间内。
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

