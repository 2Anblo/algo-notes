# 77. 组合

## 题目描述

给定两个整数 `n` 和 `k`，返回范围 `[1, n]` 中所有可能的 `k` 个数的组合。

你可以按 **任何顺序** 返回答案。

 

**示例 1：**

```
输入：n = 4, k = 2
输出：
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

**示例 2：**

```
输入：n = 1, k = 1
输出：[[1]]
```

 

**提示：**

- `1 <= n <= 20`
- `1 <= k <= n`

## 图解思路

![image-20260510210730518](./LeetCode--代码随想录(回溯问题).assets/image-20260510210730518.png)

![image-20260510210814266](./LeetCode--代码随想录(回溯问题).assets/image-20260510210814266.png)

## 代码

```java
class Solution {

    // 存放最终结果
    List<List<Integer>> result = new ArrayList<>();

    // 存放当前路径（当前已经选择的数字）
    List<Integer> path = new LinkedList<>();

    /**
     * 回溯函数
     *
     * @param n           数字范围 [1, n]
     * @param k           每个组合中需要 k 个数
     * @param startIndex  本层递归开始的位置
     */
    public void backtracking(int n, int k, int startIndex){

        // 终止条件：
        // 当路径中的元素个数等于 k 时
        // 说明找到一个合法组合
        if(path.size() == k){

            // 注意要 new 一个新的 List
            // 否则后面 path 改变会影响 result 中的数据
            result.add(new ArrayList<>(path));
            return;
        }

        // 横向遍历：从 startIndex 开始选择数字
        // 剪枝：
        // 当前 i 被选中后，后面还需要
        // k - path.size() - 1 个元素
        // 若后面剩余元素不足，则停止遍历
        for(int i = startIndex; i <= n - (k - path.size() - 1); i++){

            // 1. 做选择
            path.add(i);

            // 2. 递归进入下一层
            // 下一层从 i+1 开始，避免重复使用前面的数字
            backtracking(n, k, i + 1);

            // 3. 回溯（撤销选择）
            // 恢复现场
            path.removeLast();
        }
    }

    public List<List<Integer>> combine(int n, int k) {

        // 从数字 1 开始搜索
        backtracking(n, k, 1);

        return result;
    }
}
```



# 216.组合总和III

## 题目描述

找出所有相加之和为 `n` 的 `k` 个数的组合，且满足下列条件：

- 只使用数字1到9
- 每个数字 **最多使用一次** 

返回 *所有可能的有效组合的列表* 。该列表不能包含相同的组合两次，组合可以以任何顺序返回。

 

**示例 1:**

```
输入: k = 3, n = 7
输出: [[1,2,4]]
解释:
1 + 2 + 4 = 7
没有其他符合的组合了。
```

**示例 2:**

```
输入: k = 3, n = 9
输出: [[1,2,6], [1,3,5], [2,3,4]]
解释:
1 + 2 + 6 = 9
1 + 3 + 5 = 9
2 + 3 + 4 = 9
没有其他符合的组合了。
```

**示例 3:**

```
输入: k = 4, n = 1
输出: []
解释: 不存在有效的组合。
在[1,9]范围内使用4个不同的数字，我们可以得到的最小和是1+2+3+4 = 10，因为10 > 1，没有有效的组合。
```

 

**提示:**

- `2 <= k <= 9`
- `1 <= n <= 60`

## 代码

```java
class Solution {

    // 存放最终结果
    List<List<Integer>> result = new ArrayList<>();

    // 存放当前组合路径
    List<Integer> path = new LinkedList<>();

    // 当前路径元素总和
    int sum = 0;

    /**
     * 回溯函数
     *
     * @param k           需要选择的数字个数
     * @param n           目标和
     * @param startIndex  当前递归开始的位置
     */
    public void backtracking(int k, int n, int startIndex){

        // 剪枝：
        // 当前总和已经大于目标值
        // 后面继续选择只会更大
        if(sum > n) return;

        // 终止条件：
        // 当已经选择 k 个数字时
        if(path.size() == k){

            // 若当前总和等于目标值
            // 说明找到一个合法组合
            if(sum == n){
                result.add(new LinkedList<>(path));
            }

            return;
        }

        // 横向遍历
        for(int i = startIndex;
            i <= 9 - (k - path.size() - 1);
            i++){

            // 剪枝：
            // 当前 i 选中后
            // 后面还需要 k - path.size() - 1 个元素
            // 若剩余元素不足，则停止遍历

            // 1. 做选择
            path.add(i);
            sum += i;

            // 2. 递归进入下一层
            // 下一层从 i+1 开始，避免重复选择
            backtracking(k, n, i + 1);

            // 3. 回溯（撤销选择）
            sum -= i;
            path.removeLast();
        }
    }

    public List<List<Integer>> combinationSum3(int k, int n) {

        // 从数字 1 开始搜索
        backtracking(k, n, 1);

        return result;
    }
}
```

