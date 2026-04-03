# 242.有效的字母异位词

## 题目描述

给定两个字符串 `s` 和 `t` ，编写一个函数来判断 `t` 是否是 `s` 的 字母异位词。

 

**示例 1:**

```
输入: s = "anagram", t = "nagaram"
输出: true
```

**示例 2:**

```
输入: s = "rat", t = "car"
输出: false
```

 

**提示:**

- `1 <= s.length, t.length <= 5 * 104`
- `s` 和 `t` 仅包含小写字母

 

**进阶:** 如果输入字符串包含 unicode 字符怎么办？你能否调整你的解法来应对这种情况？

## 代码

```java
class Solution {
    public boolean isAnagram(String s, String t) {
        // 定义S/T的字符数组
        char[] arrayS = s.toCharArray();
        char[] arrayT = t.toCharArray();
        // 定义结果记录表
        int[] result = new int[26];
        for(int i=0; i<arrayS.length; i++){
            result[arrayS[i]-'a'] ++; 
        }
        for(int j=0; j<arrayT.length; j++){
            result[arrayT[j]-'a'] --; 
        }
        for(int i=0; i<26; i++){
            // 若没有成功抵消，则返回false
            if(result[i]!=0){
                return false;
            }
        }
        return true;
    }
}
```



# 349.两个数组的交集

## 题目描述

给定两个数组 `nums1` 和 `nums2` ，返回 *它们的 交集* 。输出结果中的每个元素一定是 **唯一** 的。我们可以 **不考虑输出结果的顺序** 。

 

**示例 1：**

```
输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2]
```

**示例 2：**

```
输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[9,4]
解释：[4,9] 也是可通过的
```

 

**提示：**

- `1 <= nums1.length, nums2.length <= 1000`
- `0 <= nums1[i], nums2[i] <= 1000`



## 代码

### 方法一

```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        // 给定一个哈希数组
        boolean[] hash = new boolean[1001];
        for(int i : nums1){
            hash[i] = true;
        }
        // 结果列表
        List<Integer> resList = new ArrayList<>();
        for(int i : nums2){
            // 如果数组中有nums1添加的元素，加入结果集合
            if(hash[i]){
                // 防止2次添加
                hash[i] = false;
                resList.add(i);
            }
        }
        // 转换成数组
        int[] res = new int[resList.size()];

        for(int i=0; i<resList.size(); i++){
            res[i] = resList.get(i);
        }
        return res;

    }
}
```

### 方法二

```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        // 给定一个哈希数组
        int[] hash = new int[1001];
        for(int i : nums1){
            hash[i] = 1;
        }
        // 结果集合
        Set<Integer> resSet = new HashSet<>();
        for(int i : nums2){
            // 如果数组中有nums1添加的元素，加入结果集合
            if(hash[i] == 1){
                resSet.add(i);
            }
        }

        // 通过StreamAPI将结果转为数组
        return resSet.stream().mapToInt(Integer::intValue).toArray();

    }
}

```

# 454.四数相加 II（哈希表）

## 题目描述

给你四个整数数组 nums1、nums2、nums3 和 nums4 ，数组长度都是 n ，请你计算有多少个元组 (i, j, k, l) 能满足：

`0 <= i, j, k, l < n`
`nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0`


示例 1：
```
输入：nums1 = [1,2], nums2 = [-2,-1], nums3 = [-1,2], nums4 = [0,2]
输出：2
解释：
两个元组如下：
1. (0, 0, 0, 1) -> nums1[0] + nums2[0] + nums3[0] + nums4[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> nums1[1] + nums2[1] + nums3[0] + nums4[0] = 2 + (-1) + (-1) + 0 = 0
```
示例 2：
```
输入：nums1 = [0], nums2 = [0], nums3 = [0], nums4 = [0]
输出：1
```

  提示：
```
n == nums1.length
n == nums2.length
n == nums3.length
n == nums4.length
1 <= n <= 200
-228 <= nums1[i], nums2[i], nums3[i], nums4[i] <= 228
```

## 代码
```java
class Solution {
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        // 新建map,以 sum为key, count为value
        Map<Integer, Integer> map = new HashMap<>();
        // 初始化返回结果
        int result = 0;
        // 遍历nums1和nums2求和
        for(int i : nums1){
            for(int j : nums2){
                int sum = i + j;
                // 若不存在value会返回默认值0
                int count = map.getOrDefault(sum, 0) + 1;
                map.put(sum, count);
            }
        }

        // 遍历nums3和nums4求和
        for(int i : nums3){
            for(int j : nums4){
                int sum = i + j;
                // 找是否在map中有相反数,若不存在则返回0
                int count = map.getOrDefault(0-sum, 0);
                result += count;
            }
        }

        return result;

    }
}
```

# 1.两数之和

## 题目描述

给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** *`target`* 的那 **两个** 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案，并且你不能使用两次相同的元素。

你可以按任意顺序返回答案。

 

**示例 1：**

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

**示例 2：**

```
输入：nums = [3,2,4], target = 6
输出：[1,2]
```

**示例 3：**

```
输入：nums = [3,3], target = 6
输出：[0,1]
```

 

**提示：**

- `2 <= nums.length <= 104`
- `-109 <= nums[i] <= 109`
- `-109 <= target <= 109`
- **只会存在一个有效答案**

 

**进阶：**你可以想出一个时间复杂度小于 `O(n2)` 的算法吗？

## 代码

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        // 存放出现过的元素
        Map<Integer,Integer> map = new HashMap();
        int[] result = new int[2];
        for(int i=0; i<nums.length; i++){
            // 找与当前值互补的值是否在map中
            // 因为同一个key不会在map中出现两次
            // 所以例如[3,3]的情况在存进去之前就能成功判断
            int temp = target - nums[i];
            if(map.containsKey(temp)){
                result[0] = map.get(temp);
                result[1] = i;
                return result;
            } else{
                // 将entry插入map中
                map.put(nums[i],i);
            }
        }
        return null;
    }
}
```

