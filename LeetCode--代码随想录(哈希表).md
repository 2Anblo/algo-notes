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

