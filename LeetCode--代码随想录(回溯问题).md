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

# 78.子集

## 题目描述

给你一个整数数组 `nums` ，数组中的元素 **互不相同** 。返回该数组所有可能的子集（幂集）。

解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回解集。

 

**示例 1：**

```
输入：nums = [1,2,3]
输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

**示例 2：**

```
输入：nums = [0]
输出：[[],[0]]
```

 

**提示：**

- `1 <= nums.length <= 10`
- `-10 <= nums[i] <= 10`
- `nums` 中的所有元素 **互不相同**

## 代码

```java
class Solution {

    // 存放最终所有子集结果
    List<List<Integer>> result = new ArrayList<>();

    // 当前正在构造的子集路径
    // 例如：[1,2]
    LinkedList<Integer> path = new LinkedList<>();

    /**
     * 回溯函数
     *
     * @param nums 原数组
     * @param startIndex 当前开始选择的位置
     */
    void backtracking(int[] nums, int startIndex){

        // 每到一个节点，都把当前路径加入结果
        // 因为子集问题：树上的每个节点都是一个合法子集
        result.add(new LinkedList<>(path));

        // 从 startIndex 开始，避免重复选择前面的元素
        for(int i = startIndex; i < nums.length; i++){

            // 选择当前元素
            path.add(nums[i]);

            // 递归进入下一层
            // 下一层从 i+1 开始选
            backtracking(nums, i + 1);

            // 回溯：撤销选择
            path.removeLast();
        }
    }

    public List<List<Integer>> subsets(int[] nums) {

        // 从下标 0 开始搜索
        backtracking(nums, 0);

        return result;
    }
}
```

# 90.子集II

## 题目描述

给你一个整数数组 `nums` ，其中可能包含重复元素，请你返回该数组所有可能的 子集（幂集）。

解集 **不能** 包含重复的子集。返回的解集中，子集可以按 **任意顺序** 排列。

 

**示例 1：**

```
输入：nums = [1,2,2]
输出：[[],[1],[1,2],[1,2,2],[2],[2,2]]
```

**示例 2：**

```
输入：nums = [0]
输出：[[],[0]]
```

 

**提示：**

- `1 <= nums.length <= 10`
- `-10 <= nums[i] <= 10`

 

## 代码

```java
class Solution {

    // 存放最终结果
    List<List<Integer>> result = new ArrayList<>();

    // 当前路径（当前子集）
    List<Integer> path = new LinkedList<>();


    /**
     * 回溯函数
     *
     * @param nums 排序后的数组
     * @param startIndex 本层递归开始的位置
     * @param used 记录当前路径中哪些元素被使用过
     */
    public void backtracking(int[] nums, int startIndex, int[] used){

        // 每到一个节点，都把当前路径加入结果
        result.add(new LinkedList<>(path));

        // 横向遍历（同一树层）
        for(int i = startIndex; i < nums.length; i++){

            /**
             * 树层去重（核心）
             *
             * nums[i] == nums[i - 1]
             *      说明当前元素和前一个元素相同
             *
             * used[i - 1] == 0
             *      说明前一个元素已经回溯结束
             *      即：
             *      前一个元素和当前元素处于同一树层
             *
             * 同一层中，相同元素只取第一个
             */
            if(i > startIndex
                    && nums[i - 1] == nums[i]
                    && used[i - 1] == 0){
                continue;
            }

            // 做选择
            path.add(nums[i]);

            // 标记当前元素已使用（进入树枝）
            used[i] = 1;

            // 递归下一层
            backtracking(nums, i + 1, used);

            // 回溯：撤销选择
            used[i] = 0;

            // 删除当前路径最后一个元素
            path.removeLast();
        }
    }

    public List<List<Integer>> subsetsWithDup(int[] nums) {

        // 必须先排序
        // 这样相同元素才会相邻，才能进行去重
        Arrays.sort(nums);

        // used数组：
        // 1 表示当前元素在当前路径中
        // 0 表示当前元素不在当前路径中
        int[] used = new int[nums.length];

        backtracking(nums, 0, used);

        return result;
    }
}
```

# 491.递增子序列

## 题目描述

给你一个整数数组 `nums` ，找出并返回所有该数组中不同的递增子序列，递增子序列中 **至少有两个元素** 。你可以按 **任意顺序** 返回答案。

数组中可能含有重复元素，如出现两个整数相等，也可以视作递增序列的一种特殊情况。

 

**示例 1：**

```
输入：nums = [4,6,7,7]
输出：[[4,6],[4,6,7],[4,6,7,7],[4,7],[4,7,7],[6,7],[6,7,7],[7,7]]
```

**示例 2：**

```
输入：nums = [4,4,3,2,1]
输出：[[4,4]]
```

 

**提示：**

- `1 <= nums.length <= 15`
- `-100 <= nums[i] <= 100`

## 代码

```java
class Solution {

    // 存放最终结果
    List<List<Integer>> result = new ArrayList<>();

    // 当前递归路径（当前子序列）
    List<Integer> path = new ArrayList<>();


    /**
     * 回溯函数
     *
     * @param nums 原数组
     * @param startIndex 当前层开始搜索的位置
     */
    public void backtracking(int[] nums, int startIndex){

        // 题目要求子序列长度至少为2
        // 满足条件就加入结果集
        if(path.size() > 1){
            result.add(new ArrayList<>(path));
        }

        /**
         * 本层去重集合
         *
         * 作用：
         * 同一树层中，相同数字只使用一次
         *
         * 注意：
         * 这里不能使用全局 used[]
         * 因为本题不能排序，
         * 相同元素不一定相邻
         */
        Set<Integer> used = new HashSet<>();


        // 横向遍历（树层）
        for(int i = startIndex; i < nums.length; i++){

            /**
             * 剪枝1：树层去重
             *
             * 如果当前数字在本层已经使用过，
             * 则跳过，避免重复结果
             */
            if(used.contains(nums[i])) continue;


            /**
             * 剪枝2：保证递增
             *
             * 如果当前数字小于路径最后一个数字，
             * 则不满足递增条件
             */
            if(!path.isEmpty()
                    && nums[i] < path.get(path.size() - 1)){
                continue;
            }


            // 本层标记已使用
            used.add(nums[i]);

            // 做选择
            path.add(nums[i]);

            // 递归下一层
            backtracking(nums, i + 1);

            // 回溯：撤销选择
            path.remove(path.size() - 1);
        }
    }


    public List<List<Integer>> findSubsequences(int[] nums) {

        // 从下标0开始搜索
        backtracking(nums, 0);

        return result;
    }
}
```

# 46.全排列

## 题目描述

给定一个不含重复数字的数组 `nums` ，返回其 *所有可能的全排列* 。你可以 **按任意顺序** 返回答案。

 

**示例 1：**

```
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**示例 2：**

```
输入：nums = [0,1]
输出：[[0,1],[1,0]]
```

**示例 3：**

```
输入：nums = [1]
输出：[[1]]
```

 

**提示：**

- `1 <= nums.length <= 6`
- `-10 <= nums[i] <= 10`
- `nums` 中的所有整数 **互不相同**

## 代码

```java
class Solution {

    // 存放所有排列结果
    List<List<Integer>> result = new ArrayList<>();

    // 当前路径（当前排列）
    List<Integer> path = new ArrayList<>();


    /**
     * 回溯函数
     *
     * @param nums 原数组
     * @param used used[i] 表示 nums[i] 是否已经被使用
     */
    public void backtracking(int[] nums, int[] used){

        /**
         * 终止条件
         *
         * 当当前路径长度等于数组长度时，
         * 说明已经形成一个完整排列
         */
        if(path.size() == nums.length){

            // 加入结果集
            result.add(new ArrayList<>(path));

            return;
        }


        /**
         * 全排列：
         * 每一层都要从 0 开始遍历
         *
         * 因为：
         * 每个位置都可以放任意未使用元素
         */
        for(int i = 0; i < nums.length; i++){

            /**
             * 如果当前元素没有被使用
             */
            if(used[i] == 0){

                // 做选择：加入当前元素
                path.add(nums[i]);

                // 标记当前元素已使用
                used[i] = 1;

                // 递归下一层
                backtracking(nums, used);

                // 回溯：恢复现场

                // 当前元素恢复未使用状态
                used[i] = 0;

                // 删除路径最后一个元素
                path.remove(path.size() - 1);
            }
        }
    }


    public List<List<Integer>> permute(int[] nums) {

        /**
         * used数组：
         *
         * used[i] == 1
         *      表示 nums[i] 已经在当前路径中
         *
         * used[i] == 0
         *      表示 nums[i] 还未使用
         */
        int[] used = new int[nums.length];

        backtracking(nums, used);

        return result;
    }
}
```

# 47.全排列 II

## 题目描述

给定一个可包含重复数字的序列 `nums` ，***按任意顺序*** 返回所有不重复的全排列。

 

**示例 1：**

```
输入：nums = [1,1,2]
输出：
[[1,1,2],
 [1,2,1],
 [2,1,1]]
```

**示例 2：**

```
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

 

**提示：**

- `1 <= nums.length <= 8`
- `-10 <= nums[i] <= 10`

## 代码

```java
class Solution {

    // 存放最终所有不重复排列结果
    List<List<Integer>> result = new ArrayList<>();

    // 当前路径（当前排列）
    List<Integer> path = new ArrayList<>();


    /**
     * 回溯函数
     *
     * @param nums 排序后的数组
     * @param used used[i] 表示 nums[i] 是否已经被使用
     */
    public void backtracking(int[] nums, int[] used){

        /**
         * 终止条件
         *
         * 当当前路径长度等于数组长度时，
         * 说明已经形成一个完整排列
         */
        if(path.size() == nums.length){

            // 加入结果集
            result.add(new ArrayList<>(path));

            return;
        }


        /**
         * 全排列：
         * 每一层都从 0 开始搜索
         */
        for(int i = 0; i < nums.length; i++){

            /**
             * 树层去重（核心）
             *
             * nums[i] == nums[i - 1]
             *      说明当前元素和前一个元素相同
             *
             * used[i - 1] == 0
             *      说明前一个元素已经回溯结束，
             *      即：
             *      当前两个相同元素处于同一树层
             *
             * 同一层中，相同元素只使用一次
             */
            if(i > 0
                    && nums[i] == nums[i - 1]
                    && used[i - 1] == 0){
                continue;
            }


            /**
             * 如果当前元素还未使用
             */
            if(used[i] == 0){

                // 做选择：加入当前元素
                path.add(nums[i]);

                // 标记当前元素已使用
                used[i] = 1;

                // 递归下一层
                backtracking(nums, used);

                // 回溯：恢复现场

                // 当前元素恢复未使用状态
                used[i] = 0;

                // 删除路径最后一个元素
                path.remove(path.size() - 1);
            }
        }
    }


    public List<List<Integer>> permuteUnique(int[] nums) {

        /**
         * 必须先排序
         *
         * 这样相同元素才能相邻，
         * 后续树层去重才能生效
         */
        Arrays.sort(nums);


        /**
         * used数组：
         *
         * used[i] == 1
         *      nums[i] 已经在当前路径中
         *
         * used[i] == 0
         *      nums[i] 还未使用
         */
        int[] used = new int[nums.length];

        backtracking(nums, used);

        return result;
    }
}
```

# 51. N皇后

## 题目描述

按照国际象棋的规则，皇后可以攻击与之处在同一行或同一列或同一斜线上的棋子。

**n 皇后问题** 研究的是如何将 `n` 个皇后放置在 `n×n` 的棋盘上，并且使皇后彼此之间不能相互攻击。

给你一个整数 `n` ，返回所有不同的 **n 皇后问题** 的解决方案。

每一种解法包含一个不同的 **n 皇后问题** 的棋子放置方案，该方案中 `'Q'` 和 `'.'` 分别代表了皇后和空位。

 

**示例 1：**

![img](./LeetCode--代码随想录(回溯问题).assets/queens.jpg)

```
输入：n = 4
输出：[[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
解释：如上图所示，4 皇后问题存在两个不同的解法。
```

**示例 2：**

```
输入：n = 1
输出：[["Q"]]
```

 

**提示：**

- `1 <= n <= 9`

## 代码

```java
class Solution {

    // 存放最终结果
    // 每一个 List<String> 表示一种棋盘摆放方案
    List<List<String>> result = new ArrayList<>();

    // 当前棋盘路径
    // 每个 String 表示棋盘的一行
    List<String> path = new ArrayList<>();


    /**
     * 判断当前位置是否可以放皇后
     *
     * @param n 棋盘大小
     * @param row 当前行
     * @param column 当前列
     * @param chessboard 当前棋盘
     */
    public boolean isValid(int n, int row, int column, char[][] chessboard){

        /**
         * 检查同行、同列是否有皇后
         */
        for(int i = 0; i < row; i++){
            // 纵向检查
            if(chessboard[i][column] == 'Q') return false;
        }


        /**
         * 检查主对角线（\）
         */

        // 左上方向
        int row1 = row;
        int column1 = column;

        while(row1-- > 0 && column1-- > 0){
            if(chessboard[row1][column1] == 'Q') return false;
        }




        /**
         * 检查副对角线（/）
         */

        // 右上方向
        row1 = row;
        column1 = column;

        while(row1-- > 0 && column1++ < n - 1){
            if(chessboard[row1][column1] == 'Q') return false;
        }

        // 当前位置合法
        return true;
    }


    /**
     * 回溯函数
     *
     * @param n 棋盘大小
     * @param row 当前处理到第几行
     * @param chessboard 当前棋盘状态
     */
    public void backtracking(int n, int row, char[][] chessboard){

        /**
         * 终止条件
         *
         * row == n
         * 说明前 n 行已经全部放置完成
         */
        if(row == n){

            // 保存当前方案
            result.add(new ArrayList<>(path));

            return;
        }


        /**
         * 枚举当前行的每一列
         */
        for(int i = 0; i < n; i++){

            /**
             * 判断当前位置是否合法
             */
            if(isValid(n, row, i, chessboard)){

                // 放置皇后
                chessboard[row][i] = 'Q';

                // 当前行加入路径
                path.add(new String(chessboard[row]));

                // 递归下一行
                backtracking(n, row + 1, chessboard);

                // 回溯：移除皇后
                chessboard[row][i] = '.';

                // 删除当前行
                path.remove(path.size() - 1);
            }
        }
    }


    public List<List<String>> solveNQueens(int n) {

        /**
         * 初始化棋盘
         *
         * '.' 表示空位
         */
        char[][] chessboard = new char[n][n];

        for(int i = 0; i < n; i++){
            for(int j = 0; j < n; j++){
                chessboard[i][j] = '.';
            }
        }

        // 从第0行开始回溯
        backtracking(n, 0, chessboard);

        return result;
    }
}
```

# 52. N皇后 II

## 题目描述

**n 皇后问题** 研究的是如何将 `n` 个皇后放置在 `n × n` 的棋盘上，并且使皇后彼此之间不能相互攻击。

给你一个整数 `n` ，返回 **n 皇后问题** 不同的解决方案的数量。

 

**示例 1：**

![img](./LeetCode--代码随想录(回溯问题).assets/queens-1778680196490-3.jpg)

```
输入：n = 4
输出：2
解释：如上图所示，4 皇后问题存在两个不同的解法。
```

**示例 2：**

```
输入：n = 1
输出：1
```

 

**提示：**

- `1 <= n <= 9`

## 代码

```java
class Solution {

    // 记录最终合法方案总数
    int total = 0;


    /**
     * 判断当前位置是否可以放皇后
     *
     * @param n 棋盘大小
     * @param row 当前行
     * @param column 当前列
     * @param chessboard 当前棋盘状态
     */
    public boolean isValid(int n, int row, int column, char[][] chessboard){

        /**
         * 检查当前列上方是否已有皇后
         *
         * 因为是按行递归：
         * 当前行以下还没有放皇后，
         * 所以只需要检查上方即可
         */
        for(int i = 0; i < row; i++){

            if(chessboard[i][column] == 'Q'){
                return false;
            }
        }


        /**
         * 检查左上对角线（\）
         */
        int row1 = row;
        int column1 = column;

        while(row1-- > 0 && column1-- > 0){

            if(chessboard[row1][column1] == 'Q'){
                return false;
            }
        }


        /**
         * 检查右上对角线（/）
         */
        row1 = row;
        column1 = column;

        while(row1-- > 0 && column1++ < n - 1){

            if(chessboard[row1][column1] == 'Q'){
                return false;
            }
        }


        // 当前位置合法
        return true;
    }


    /**
     * 回溯函数
     *
     * @param n 棋盘大小
     * @param row 当前处理到第几行
     * @param chessboard 当前棋盘状态
     */
    public void backtracking(int n, int row, char[][] chessboard){

        /**
         * 终止条件
         *
         * row == n
         * 说明前 n 行已经全部放置完成
         */
        if(row == n){

            // 找到一种合法方案
            total++;

            return;
        }


        /**
         * 枚举当前行每一列
         */
        for(int i = 0; i < n; i++){

            /**
             * 如果当前位置合法
             */
            if(isValid(n, row, i, chessboard)){

                // 放置皇后
                chessboard[row][i] = 'Q';

                // 递归下一行
                backtracking(n, row + 1, chessboard);

                // 回溯：恢复现场
                chessboard[row][i] = '.';
            }
        }
    }


    public int totalNQueens(int n) {

        /**
         * 初始化棋盘
         *
         * '.' 表示空位
         */
        char[][] chessboard = new char[n][n];

        for(int i = 0; i < n; i++){
            for(int j = 0; j < n; j++){
                chessboard[i][j] = '.';
            }
        }


        // 从第0行开始回溯
        backtracking(n, 0, chessboard);

        return total;
    }
}
```

# 37. 解数独

## 题目描述

编写一个程序，通过填充空格来解决数独问题。

数独的解法需 **遵循如下规则**：

1. 数字 `1-9` 在每一行只能出现一次。
2. 数字 `1-9` 在每一列只能出现一次。
3. 数字 `1-9` 在每一个以粗实线分隔的 `3x3` 宫内只能出现一次。（请参考示例图）

数独部分空格内已填入了数字，空白格用 `'.'` 表示。

 

**示例 1：**

![img](./LeetCode--代码随想录(回溯问题).assets/250px-sudoku-by-l2g-20050714svg.png)

```
输入：board = [["5","3",".",".","7",".",".",".","."],["6",".",".","1","9","5",".",".","."],[".","9","8",".",".",".",".","6","."],["8",".",".",".","6",".",".",".","3"],["4",".",".","8",".","3",".",".","1"],["7",".",".",".","2",".",".",".","6"],[".","6",".",".",".",".","2","8","."],[".",".",".","4","1","9",".",".","5"],[".",".",".",".","8",".",".","7","9"]]
输出：[["5","3","4","6","7","8","9","1","2"],["6","7","2","1","9","5","3","4","8"],["1","9","8","3","4","2","5","6","7"],["8","5","9","7","6","1","4","2","3"],["4","2","6","8","5","3","7","9","1"],["7","1","3","9","2","4","8","5","6"],["9","6","1","5","3","7","2","8","4"],["2","8","7","4","1","9","6","3","5"],["3","4","5","2","8","6","1","7","9"]]
解释：输入的数独如上图所示，唯一有效的解决方案如下所示：
```

 

**提示：**

- `board.length == 9`
- `board[i].length == 9`
- `board[i][j]` 是一位数字或者 `'.'`
- 题目数据 **保证** 输入数独仅有一个解

## 代码

```java
class Solution {

    /**
     * 判断当前位置填入数字 k 是否合法
     *
     * @param row 当前行
     * @param column 当前列
     * @param k 当前尝试填入的数字
     * @param board 当前数独棋盘
     */
    public boolean isValid(int row, int column, char k, char[][] board){

        /**
         * 检查同行、同列
         *
         * 数独规则：
         * 同一行、同一列不能出现重复数字
         */
        for(int i = 0; i < 9; i++){

            // 同行存在重复
            if(board[row][i] == k){
                return false;
            }

            // 同列存在重复
            if(board[i][column] == k){
                return false;
            }
        }


        /**
         * 检查 3 × 3 九宫格
         */

        // 当前九宫格左上角坐标
        int rowStart = row / 3 * 3;
        int columnStart = column / 3 * 3;

        // 遍历当前九宫格
        for(int i = rowStart; i < rowStart + 3; i++){

            for(int j = columnStart; j < columnStart + 3; j++){

                // 九宫格中存在重复数字
                if(board[i][j] == k){
                    return false;
                }
            }
        }

        // 当前位置合法
        return true;
    }


    /**
     * 回溯函数
     *
     * 返回值：
     * true  -> 找到正确解
     * false -> 当前路径失败
     */
    public boolean backtracking(char[][] board){

        /**
         * 遍历整个棋盘
         */
        for(int i = 0; i < 9; i++){

            for(int j = 0; j < 9; j++){

                /**
                 * 找到空格
                 */
                if(board[i][j] == '.'){

                    /**
                     * 尝试填入 1 ~ 9
                     */
                    for(char k = '1'; k <= '9'; k++){

                        /**
                         * 如果当前数字合法
                         */
                        if(isValid(i, j, k, board)){

                            // 做选择：填入数字
                            board[i][j] = k;

                            // 递归下一层
                            boolean result = backtracking(board);

                            /**
                             * 如果后续成功，
                             * 直接返回 true
                             */
                            if(result == true){
                                return true;
                            }

                            /**
                             * 回溯：
                             * 当前数字不行，恢复为空格
                             */
                            board[i][j] = '.';
                        }
                    }

                    /**
                     * 1~9 都尝试失败
                     *
                     * 说明当前路径无解
                     */
                    return false;
                }
            }
        }

        /**
         * 没有找到空格
         *
         * 说明整个棋盘已经填完
         * 找到正确答案
         */
        return true;
    }


    public void solveSudoku(char[][] board) {

        // 启动回溯
        backtracking(board);
    }
}
```

