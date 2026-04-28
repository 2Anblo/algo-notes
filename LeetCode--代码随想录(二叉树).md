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

