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

# 17.电话号码的字母组合

## 题目描述

给定一个仅包含数字 `2-9` 的字符串，返回所有它能表示的字母组合。答案可以按 **任意顺序** 返回。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

![img](./LeetCode--代码随想录(回溯问题).assets/1752723054-mfIHZs-image.png)

 

**示例 1：**

```
输入：digits = "23"
输出：["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

**示例 2：**

```
输入：digits = "2"
输出：["a","b","c"]
```

 

**提示：**

- `1 <= digits.length <= 4`
- `digits[i]` 是范围 `['2', '9']` 的一个数字。

## 代码

```java
class Solution {

    // 存放最终结果
    List<String> result = new ArrayList<>();

    // 用来拼接当前组合
    StringBuilder sb = new StringBuilder();

    // 数字与字母的映射关系
    // 下标代表数字
    // 例如：2 -> "abc"
    String[] letterMap = new String[]{
        "", "", "abc", "def", "ghi",
        "jkl", "mno", "pqrs", "tuv", "wxyz"
    };

    /**
     * 回溯函数
     *
     * @param nums        输入数字数组
     * @param startIndex  当前处理到第几个数字
     */
    public void backtracking(int[] nums, int startIndex){

        // 终止条件：
        // 如果当前字符串长度已经等于数字个数
        // 说明已经完成一次组合
        if(sb.length() == nums.length){
            result.add(sb.toString());
            return;
        }

        // 获取当前数字对应的字母
        String letters = letterMap[nums[startIndex]];

        // 转成字符数组方便遍历
        char[] chArray = letters.toCharArray();

        // 遍历当前数字对应的所有字母
        for(int i = 0; i < chArray.length; i++){

            // 选择当前字母
            sb.append(chArray[i]);

            // 递归处理下一个数字
            backtracking(nums, startIndex + 1);

            // 回溯：撤销当前选择
            sb.deleteCharAt(sb.length() - 1);
        }
    }

    public List<String> letterCombinations(String digits) {

        // 特殊情况：空字符串直接返回空结果
        if(digits == null || digits.length() == 0){
            return result;
        }

        // 将字符串数字转成 int 数组
        int[] nums = new int[digits.length()];

        char[] chs = digits.toCharArray();

        for(int i = 0; i < nums.length; i++){
            nums[i] = chs[i] - '0';
        }

        // 开始回溯
        backtracking(nums, 0);

        return result;
    }
}
```

# 39. 组合总和

## 题目描述

给你一个 **无重复元素** 的整数数组 `candidates` 和一个目标整数 `target` ，找出 `candidates` 中可以使数字和为目标数 `target` 的 所有 **不同组合** ，并以列表形式返回。你可以按 **任意顺序** 返回这些组合。

`candidates` 中的 **同一个** 数字可以 **无限制重复被选取** 。如果至少一个数字的被选数量不同，则两种组合是不同的。 

对于给定的输入，保证和为 `target` 的不同组合数少于 `150` 个。

 

**示例 1：**

```
输入：candidates = [2,3,6,7], target = 7
输出：[[2,2,3],[7]]
解释：
2 和 3 可以形成一组候选，2 + 2 + 3 = 7 。注意 2 可以使用多次。
7 也是一个候选， 7 = 7 。
仅有这两种组合。
```

**示例 2：**

```
输入: candidates = [2,3,5], target = 8
输出: [[2,2,2,2],[2,3,3],[3,5]]
```

**示例 3：**

```
输入: candidates = [2], target = 1
输出: []
```

 

**提示：**

- `1 <= candidates.length <= 30`
- `2 <= candidates[i] <= 40`
- `candidates` 的所有元素 **互不相同**
- `1 <= target <= 40`

##  代码

```java
class Solution {

    // 存放最终结果
    List<List<Integer>> result = new ArrayList<>();

    // 当前路径（当前组合）
    List<Integer> path = new LinkedList<>();

    // 当前路径的总和
    int sum = 0;

    /**
     * 回溯函数
     *
     * @param candidates  候选数组
     * @param target      目标和
     * @param startIndex  当前开始遍历的位置
     */
    public void backtracking(int[] candidates, int target, int startIndex){

        // 终止条件：
        // 如果当前和等于目标值
        // 说明找到一个合法组合
        if(sum == target){
            result.add(new ArrayList<>(path));
            return;
        }

        // 从 startIndex 开始遍历
        for(int i = startIndex;
            i < candidates.length && sum + candidates[i] <= target;
            i++){

            // 剪枝：
            // 因为数组已经排序
            // 如果当前值加入后已经超过 target
            // 后面的值只会更大，因此直接停止循环

            // 选择当前元素
            sum += candidates[i];
            path.add(candidates[i]);

            // 递归
            // i 不加 1，表示元素可以重复使用
            backtracking(candidates, target, i);

            // 回溯
            // 撤销本次选择
            path.removeLast();
            sum -= candidates[i];
        }
    }

    public List<List<Integer>> combinationSum(int[] candidates, int target) {

        // 先排序，方便后续剪枝
        Arrays.sort(candidates);

        // 开始回溯
        backtracking(candidates, target, 0);

        return result;
    }
}
```

# 40.组合总和 II

## 题目描述

给定一个候选人编号的集合 `candidates` 和一个目标数 `target` ，找出 `candidates` 中所有可以使数字和为 `target` 的组合。

`candidates` 中的每个数字在每个组合中只能使用 **一次** 。

**注意：**解集不能包含重复的组合。 

 

**示例 1:**

```
输入: candidates = [10,1,2,7,6,1,5], target = 8,
输出:
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]
```

**示例 2:**

```
输入: candidates = [2,5,2,1,2], target = 5,
输出:
[
[1,2,2],
[5]
]
```

 

**提示:**

- `1 <= candidates.length <= 100`
- `1 <= candidates[i] <= 50`
- `1 <= target <= 30`

## 图解思路

理解树层去重思想：

![image-20260511152450684](./LeetCode--代码随想录(回溯问题).assets/image-20260511152450684.png)

## 代码

```java
class Solution {

    // 最终结果集
    List<List<Integer>> result = new ArrayList<>();

    // 当前组合路径
    List<Integer> path = new LinkedList<>();

    // 当前路径元素和
    int sum = 0;

    /**
     * 回溯搜索所有满足条件的组合
     *
     * @param candidates 候选数组（已排序）
     * @param target     目标和
     * @param startIndex 本层搜索起点
     * @param used       记录当前元素是否已被使用
     */
    public void backtracking(int[] candidates, int target,
                             int startIndex, int[] used) {

        // 找到满足条件的组合
        if (sum == target) {
            result.add(new ArrayList<>(path));
            return;
        }

        // 横向遍历本层节点
        //
        // 剪枝：
        // sum + candidates[i] > target 时，
        // 后续元素更大，必然也不满足
        for (int i = startIndex;
             i < candidates.length && sum + candidates[i] <= target;
             i++) {

            /**
             * 树层去重（核心）
             *
             * candidates[i] == candidates[i - 1]
             * 说明当前元素与前一个元素重复
             *
             * used[i - 1] == 0
             * 说明前一个相同元素是在“同一树层”中被撤销的
             *
             * 即：
             * 本层已经使用过相同数字，
             * 再选当前数字会产生重复组合
             *
             * 因此跳过
             */
            if (i > 0
                && candidates[i] == candidates[i - 1]
                && used[i - 1] == 0) {
                continue;
            }

            // 选择当前节点
            sum += candidates[i];
            path.add(candidates[i]);
            used[i] = 1;

            // 递归下一层
            //
            // i + 1 表示：
            // 每个元素只能使用一次
            backtracking(candidates, target, i + 1, used);

            // 回溯：撤销选择
            used[i] = 0;
            path.removeLast();
            sum -= candidates[i];
        }
    }

    public List<List<Integer>> combinationSum2(int[] candidates,
                                               int target) {

        // 排序是去重的前提
        //
        // 相同元素必须相邻，
        // 才能通过 candidates[i] == candidates[i - 1] 去重
        Arrays.sort(candidates);

        int[] used = new int[candidates.length];

        backtracking(candidates, target, 0, used);

        return result;
    }
}
```

# 131.分割回文串

## 题目描述

给你一个字符串 `s`，请你将 `s` 分割成一些 子串，使每个子串都是 **回文串** 。返回 `s` 所有可能的分割方案。

 

**示例 1：**

```
输入：s = "aab"
输出：[["a","a","b"],["aa","b"]]
```

**示例 2：**

```
输入：s = "a"
输出：[["a"]]
```

 

**提示：**

- `1 <= s.length <= 16`
- `s` 仅由小写英文字母组成

## 代码

```java
class Solution {

    // 存放最终结果
    // result 中的每一个 List<String> 都是一种分割方案
    List<List<String>> result = new ArrayList<>();

    // 用来记录当前路径（当前已经切割出的回文子串）
    List<String> path = new LinkedList<>();


    /**
     * 回溯函数
     *
     * @param s 原字符串
     * @param startIndex 当前切割开始的位置
     */
    void backtracking(String s, int startIndex){

        // 终止条件：
        // 如果 startIndex 来到了字符串末尾
        // 说明已经完成了一种切割方案
        if(startIndex == s.length()){
            result.add(new ArrayList<>(path));
            return;
        }

        // 从 startIndex 开始尝试切割
        for(int i = startIndex; i < s.length(); i++){

            // 判断 s[startIndex ~ i] 是否为回文串
            if(isPalindrome(s, startIndex, i)){

                // 如果是回文串，则加入当前路径
                path.add(s.substring(startIndex, i + 1));

                // 继续递归，下一次从 i+1 开始切割
                backtracking(s, i + 1);

                // 回溯：撤销本次选择
                path.removeLast();
            }
        }
    }


    /**
     * 判断一个字符串区间是否为回文串
     *
     * @param s 原字符串
     * @param start 左指针
     * @param end 右指针
     * @return 是否为回文串
     */
    boolean isPalindrome(String s, int start, int end){

        // 双指针向中间靠拢
        while(start <= end){

            // 只要有一个字符不同，就不是回文串
            if(s.charAt(start++) != s.charAt(end--)){
                return false;
            }
        }

        // 全部字符都相同，说明是回文串
        return true;
    }


    /**
     * 主函数
     * 返回字符串所有可能的回文分割方案
     */
    public List<List<String>> partition(String s) {

        // 从下标 0 开始回溯
        backtracking(s, 0);

        return result;
    }
}
```

# 93.复原IP地址

## 题目描述

**有效 IP 地址** 正好由四个整数（每个整数位于 `0` 到 `255` 之间组成，且不能含有前导 `0`），整数之间用 `'.'` 分隔。

- 例如：`"0.1.2.201"` 和` "192.168.1.1"` 是 **有效** IP 地址，但是 `"0.011.255.245"`、`"192.168.1.312"` 和 `"192.168@1.1"` 是 **无效** IP 地址。

给定一个只包含数字的字符串 `s` ，用以表示一个 IP 地址，返回所有可能的**有效 IP 地址**，这些地址可以通过在 `s` 中插入 `'.'` 来形成。你 **不能** 重新排序或删除 `s` 中的任何数字。你可以按 **任何** 顺序返回答案。

 

**示例 1：**

```
输入：s = "25525511135"
输出：["255.255.11.135","255.255.111.35"]
```

**示例 2：**

```
输入：s = "0000"
输出：["0.0.0.0"]
```

**示例 3：**

```
输入：s = "101023"
输出：["1.0.10.23","1.0.102.3","10.1.0.23","10.10.2.3","101.0.2.3"]
```

 

**提示：**

- `1 <= s.length <= 20`
- `s` 仅由数字组成

##  代码

```java
class Solution {

    // 存放最终所有合法 IP 地址
    List<String> result = new ArrayList<>();

    // 用来拼接当前构造中的 IP 地址
    StringBuilder sb = new StringBuilder();

    // 记录当前已经添加了几个点
    // IP 地址一共需要 3 个点
    int count = 0;


    /**
     * 判断当前字符串是否为合法 IP 段
     */
    public boolean isValid(String s){

        // 不能有前导 0
        // 例如 "01"、"001" 都不合法
        if(s.startsWith("0") && s.length() > 1) return false;

        // IP 段长度不能超过 3
        // 也不能为空
        // 必须全为数字
        if(s.length() > 3 || s.isEmpty() || !s.matches("\\d+")) return false;

        // 转成数字判断是否超过 255
        int num = Integer.parseInt(s);

        if(num > 255){
            return false;
        }

        return true;
    }


    /**
     * 回溯函数
     *
     * @param s 原字符串
     * @param startIndex 当前切割开始位置
     */
    public void backtracking(String s, int startIndex){

        // 如果已经放了 3 个点
        // 说明前面已经分成了 3 段
        // 剩余部分直接作为最后一段
        if(count == 3){

            // 判断最后一段是否合法
            if(isValid(s.substring(startIndex, s.length()))){

                // 加入最后一段
                sb.append(s.substring(startIndex, s.length()));

                // 收集答案
                result.add(sb.toString());

                // 回溯：删除最后加入的这一段
                sb.delete(startIndex + 3, sb.length());
            }

            return;
        }


        // 枚举当前这一段的结束位置
        for(int i = startIndex; i < s.length(); i++){

            // 当前切割出的字符串
            // 范围：[startIndex, i]
            if(isValid(s.substring(startIndex, i + 1))){

                // 当前这一段长度
                int len = s.substring(startIndex, i + 1).length();

                // 加入当前段
                sb.append(s.substring(startIndex, i + 1));

                // 加入点
                sb.append(".");

                // 点数 +1
                count++;

                // 递归处理下一段
                backtracking(s, i + 1);

                // 回溯：撤销本次选择
                count--;

                // 删除：
                // 当前加入的字符串 + 点
                sb.delete(sb.length() - len - 1, sb.length());
            }
        }
    }


    /**
     * 主函数
     */
    public List<String> restoreIpAddresses(String s) {

        // 从下标 0 开始切割
        backtracking(s, 0);

        return result;
    }
}
```

