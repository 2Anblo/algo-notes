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

# 383.赎金信

## 题目描述

给你两个字符串：`ransomNote` 和 `magazine` ，判断 `ransomNote` 能不能由 `magazine` 里面的字符构成。

如果可以，返回 `true` ；否则返回 `false` 。

`magazine` 中的每个字符只能在 `ransomNote` 中使用一次。

 

**示例 1：**

```
输入：ransomNote = "a", magazine = "b"
输出：false
```

**示例 2：**

```
输入：ransomNote = "aa", magazine = "ab"
输出：false
```

**示例 3：**

```
输入：ransomNote = "aa", magazine = "aab"
输出：true
```

 

**提示：**

- `1 <= ransomNote.length, magazine.length <= 105`
- `ransomNote` 和 `magazine` 由小写英文字母组成

## 代码

```java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        
        // 边界条件判断
        if(ransomNote.length()>magazine.length()) return false;

        int[] result = new int [26];
        // 将字符串转化为字符数组
        char[] ransomArray = ransomNote.toCharArray();
        char[] magazineArray = magazine.toCharArray();


        for(char i : ransomArray){
            // 通过result表记录每个字符出现的次数
            result[i-'a']++;
        }

        for(char i : magazineArray){
            result[i-'a']--;
        }

        // 统计所有字符出现个数之差，若有ransom比magazine多则返回false
        for(int i : result){
            if(i>0) return false;
        }
        return true;

    }
}
```

# 15.三数之和

## 题目描述

给你一个整数数组 `nums` ，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j`、`i != k` 且 `j != k` ，同时还满足 `nums[i] + nums[j] + nums[k] == 0` 。请你返回所有和为 `0` 且不重复的三元组。

**注意：**答案中不可以包含重复的三元组。

 

 

**示例 1：**

```
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
解释：
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。
不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
注意，输出的顺序和三元组的顺序并不重要。
```

**示例 2：**

```
输入：nums = [0,1,1]
输出：[]
解释：唯一可能的三元组和不为 0 。
```

**示例 3：**

```
输入：nums = [0,0,0]
输出：[[0,0,0]]
解释：唯一可能的三元组和为 0 。
```

 

**提示：**

- `3 <= nums.length <= 3000`
- `-105 <= nums[i] <= 105`

## 题解

**双指针法**是不会漏解，具有高效剪枝效率的查找方法。

在使用前需要将给定**数组**进行从小到大**排序**。

![image-20260406112509942](./LeetCode--代码随想录(哈希表).assets/image-20260406112509942.png)

![image-20260406112514705](./LeetCode--代码随想录(哈希表).assets/image-20260406112514705.png)

## 代码

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        // 将数组有序排列
        Arrays.sort(nums);
        // 最终要返回的结果组
        List<List<Integer>> result = new ArrayList<>();

        // 固定一端i，左右指针遍历三元组
        for(int i=0; i<nums.length-2; i++){
            // 边界条件判断
            if(nums[i]>0) break;
            // 对下标i对应元素进行去重
            if(i>0 && nums[i]==nums[i-1]) continue;
            // 左指针
            int left = i+1;
            // 右指针
            int right = nums.length-1;
            // 双指针相向运动
            while(left < right){
                int sum = nums[i]+nums[left]+nums[right];
                // 当和大于0，right左移
                if(sum > 0){
                    right--;
                }else if(sum < 0){// 当和小于0，left右移
                    left++;
                } else{
                    result.add(Arrays.asList(nums[i],nums[left],nums[right]));

                    // 对left和right进行去重
                    while(left < right && nums[left]==nums[left+1]) left++;
                    while(left < right && nums[right]==nums[right-1]) right--;

                    // 将一组正确的解过滤
                    left++;
                    right--;
                }
            }
        }

        return result;

    }
}
```

# 18.四数之和

## 题目描述

给你一个由 `n` 个整数组成的数组 `nums` ，和一个目标值 `target` 。请你找出并返回满足下述全部条件且**不重复**的四元组 `[nums[a], nums[b], nums[c], nums[d]]` （若两个四元组元素一一对应，则认为两个四元组重复）：

- `0 <= a, b, c, d < n`
- `a`、`b`、`c` 和 `d` **互不相同**
- `nums[a] + nums[b] + nums[c] + nums[d] == target`

你可以按 **任意顺序** 返回答案 。

 

**示例 1：**

```
输入：nums = [1,0,-1,0,-2,2], target = 0
输出：[[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]
```

**示例 2：**

```
输入：nums = [2,2,2,2,2], target = 8
输出：[[2,2,2,2]]
```

 

**提示：**

- `1 <= nums.length <= 200`
- `-10^9 <= nums[i] <= 10^9`
- `-10^9 <= target <= 10^9`

## 题解

尝试复刻三数之和，使用$O(n^2)$时间复杂度解决，失败：

![image-20260406153405371](./LeetCode--代码随想录(哈希表).assets/image-20260406153405371.png)

固定两个指针，再用另外两个指针进行扫描$O(n^3)$时间复杂度解决

![image-20260406153450551](./LeetCode--代码随想录(哈希表).assets/image-20260406153450551.png)

![image-20260406153455978](./LeetCode--代码随想录(哈希表).assets/image-20260406153455978.png)

## 代码

错误解法：

```java
class Solution {
    // 这种双指针写法过于贪心，无法适用于4Sum问题
    public List<List<Integer>> fourSum(int[] nums, int target) {
        // 最终要返回的结果列表
        List<List<Integer>> result = new ArrayList<>();
        // 对数组进行排序
        Arrays.sort(nums);
        // 给定左右两个外围指针
        int leftOut = 0;
        int rightOut = nums.length - 1;
        // 两对双指针遍历数组
        while(leftOut < rightOut-2){
            // 边界条件判断
            if(nums[leftOut]>target) break;
            // 对leftOut和rightOut进行剪枝
            if(leftOut>0 && nums[leftOut]==nums[leftOut-1]){
                leftOut++;
                continue;
            }
            if(rightOut<nums.length-1 && nums[rightOut]==nums[rightOut+1]){
                rightOut--;
                continue;
            }
            // 初始化左右两个内部指针
            int leftIn = leftOut+1;
            int rightIn = rightOut-1;
            while(leftIn < rightIn){
                int sum = nums[leftIn]+nums[leftOut]+nums[rightOut]+nums[rightIn];
                if(sum > target){
                    rightIn--;
                    continue;
                } else if(sum < target){
                    leftIn++;
                    continue;
                } else{
                    // 加入结果四元组
                    result.add(Arrays.asList(nums[leftIn],nums[leftOut],nums[rightIn],nums[rightOut]));
                    // 对leftIn和rightIn去重剪枝
                    while(leftIn<rightIn && nums[leftIn]==nums[leftIn+1]) leftIn++;
                    while(leftIn<rightIn && nums[rightIn]==nums[rightIn-1]) rightIn--;
                    
                    // 过滤掉一组正确解
                    rightIn--;
                    leftIn++;
                }
            }
            if(nums[rightOut]+nums[leftOut]>=target){
                rightOut--;
            } else{
                leftOut++;
            }
            

        }

        return result;
    }
}
```

正确解法：

```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        // 最终要返回的列表
        List<List<Integer>> result = new ArrayList<>();
        // 对数组进行排序
        Arrays.sort(nums);

        // 固定前两个数，用剩余部分进行双指针扫描
        for(int i=0; i<nums.length-3; i++){
            // 边界条件判断，防止大数溢出
            long min = (long) nums[i]+nums[i+1]+nums[i+2]+nums[i+3];
            if(min > target) break;
            long max = (long) nums[i]+nums[nums.length-1]+nums[nums.length-2]+nums[nums.length-3];
            if(max < target) continue;
            // 对i去重
            if(i>0 && nums[i]==nums[i-1]) continue;
            // 固定第二个数
            for(int j=i+1; j<nums.length-2; j++){
                // 对j去重
                if(j>i+1 && nums[j]==nums[j-1]) continue;
                // 给定双指针
                int left = j+1;
                int right = nums.length-1;
                while(left<right){
                    // 防止大数溢出
                    long sum = (long)nums[i]+nums[j]+nums[left]+nums[right];
                    if(sum > target){
                        right--;
                        continue;
                    } else if(sum < target){
                        left++;
                        continue;
                    } else{
                        result.add(Arrays.asList(nums[i],nums[j],nums[left],nums[right]));
                        // 对left和right去重
                        while(left<right && nums[left]==nums[left+1]) left++;
                        while(left<right && nums[right]==nums[right-1]) right--;
                        // 过滤一组正确解
                        left++;
                        right--;
                    }
                }
            }
        }
        return result;
    }
}
```

