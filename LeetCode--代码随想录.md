# LeetCode--代码随想录

## 704.二分查找

### 题目描述

给定一个 `n` 个元素有序的（升序）整型数组 `nums` 和一个目标值 `target` ，写一个函数搜索 `nums` 中的 `target`，如果 `target` 存在返回下标，否则返回 `-1`。

你必须编写一个具有 `O(log n)` 时间复杂度的算法。

**示例 1:**

```
输入: nums = [-1,0,3,5,9,12], target = 9
输出: 4
解释: 9 出现在 nums 中并且下标为 4
```

**示例 2:**

```
输入: nums = [-1,0,3,5,9,12], target = 2
输出: -1
解释: 2 不存在 nums 中因此返回 -1
```

 

**提示：**

1. 你可以假设 `nums` 中的所有元素是不重复的。
2. `n` 将在 `[1, 10000]`之间。
3. `nums` 的每个元素都将在 `[-9999, 9999]`之间。

### 解题思路

**这道题目的前提是数组为有序数组**，同时题目还强调**数组中无重复元素**，因为一旦有重复元素，使用二分查找法返回的元素下标可能不是唯一的，这些都是使用二分法的前提条件，当大家看到题目描述满足如上条件的时候，可要想一想是不是可以用二分法了。

二分查找涉及的很多的边界条件，逻辑比较简单，但就是写不好。例如到底是 `while(left < right)` 还是 `while(left <= right)`，到底是`right = middle`呢，还是要`right = middle - 1`呢？

大家写二分法经常写乱，主要是因为**对区间的定义没有想清楚，区间的定义就是不变量**。要在二分查找的过程中，保持不变量，就是在while寻找中每一次边界的处理都要坚持根据区间的定义来操作，这就是**循环不变量**规则。

写二分法，区间的定义一般为两种，左闭右闭即[left, right]，或者左闭右开即[left, right)。

我个人倾向使用左闭右闭方法处理二分法。

如图所示，当我们使用左闭右闭即`[left, right]`时，就需要让`right = middle - 1`了。因为`middle`的值已经做过一次比较。

![image-20260321184044881](./LeetCode--代码随想录.assets/image-20260321184044881.png)

### 代码

```java
class Solution {
    public int search(int[] nums, int target) {
        
        int length = nums.length;
        // 左边下标初始为0
        int left = 0;
        // 记录右边下标，为数组长度 - 1
        int right = length - 1;
        // 避免当 target 小于nums[0] nums[nums.length - 1]时多次循环运算
        if (target < nums[0] || target > nums[length - 1]) {
            return -1;
        }
        // 使用左闭右闭即[left, right]区间
        while(left <= right){
             // 初始化中点 middle，以防溢出，用减法
             int middle = left + (right - left) / 2;
            // 比target大，right = middle - 1
            if(nums[middle] > target){
                right = middle - 1;
            } else if(nums[middle] < target){
                // 比target小，left = middle + 1
                left = middle + 1;
            } else{
                return middle;
            }
        }
        // 若left > right，要求的target数组中不存在
        return -1;
        
    }
}
```

