# 739. 每日温度

## 题目描述

给定一个整数数组 `temperatures` ，表示每天的温度，返回一个数组 `answer` ，其中 `answer[i]` 是指对于第 `i` 天，下一个更高温度出现在几天后。如果气温在这之后都不会升高，请在该位置用 `0` 来代替。

 

**示例 1:**

```
输入: temperatures = [73,74,75,71,69,72,76,73]
输出: [1,1,4,2,1,1,0,0]
```

**示例 2:**

```
输入: temperatures = [30,40,50,60]
输出: [1,1,1,0]
```

**示例 3:**

```
输入: temperatures = [30,60,90]
输出: [1,1,0]
```

 

**提示：**

- `1 <= temperatures.length <= 105`
- `30 <= temperatures[i] <= 100`

## 图解思路

![image-20260605160610883](./LeetCode--代码随想录(单调栈).assets/image-20260605160610883.png)

## 代码

```java
class Solution {
    public int[] dailyTemperatures(int[] temperatures) {

        // 单调栈：存储温度对应的下标
        // 栈内下标对应的温度保持单调递减
        Deque<Integer> st = new ArrayDeque<>();

        // 结果数组
        // result[i] 表示第 i 天还需要等待多少天才能遇到更高温度
        int[] result = new int[temperatures.length];

        // 第 0 天下标先入栈
        st.push(0);

        // 从第 1 天开始遍历
        for (int i = 1; i < temperatures.length; i++) {

            // 当前温度比栈顶对应温度高
            // 说明找到了栈顶元素的下一个更高温度
            while (!st.isEmpty() && temperatures[i] > temperatures[st.peek()]) {

                // 栈顶下标对应的答案
                // 当前下标 - 栈顶下标 = 等待天数
                result[st.peek()] = i - st.peek();

                // 已找到答案，弹出
                st.pop();
            }

            // 当前下标入栈
            // 继续等待未来更高温度出现
            st.push(i);
        }

        return result;
    }
}
```

# 496.下一个更大元素 I

## 题目描述

`nums1` 中数字 `x` 的 **下一个更大元素** 是指 `x` 在 `nums2` 中对应位置 **右侧** 的 **第一个** 比 `x` 大的元素。

给你两个 **没有重复元素** 的数组 `nums1` 和 `nums2` ，下标从 **0** 开始计数，其中`nums1` 是 `nums2` 的子集。

对于每个 `0 <= i < nums1.length` ，找出满足 `nums1[i] == nums2[j]` 的下标 `j` ，并且在 `nums2` 确定 `nums2[j]` 的 **下一个更大元素** 。如果不存在下一个更大元素，那么本次查询的答案是 `-1` 。

返回一个长度为 `nums1.length` 的数组 `ans` 作为答案，满足 `ans[i]` 是如上所述的 **下一个更大元素** 。

 

**示例 1：**

```
输入：nums1 = [4,1,2], nums2 = [1,3,4,2].
输出：[-1,3,-1]
解释：nums1 中每个值的下一个更大元素如下所述：
- 4 ，用加粗斜体标识，nums2 = [1,3,4,2]。不存在下一个更大元素，所以答案是 -1 。
- 1 ，用加粗斜体标识，nums2 = [1,3,4,2]。下一个更大元素是 3 。
- 2 ，用加粗斜体标识，nums2 = [1,3,4,2]。不存在下一个更大元素，所以答案是 -1 。
```

**示例 2：**

```
输入：nums1 = [2,4], nums2 = [1,2,3,4].
输出：[3,-1]
解释：nums1 中每个值的下一个更大元素如下所述：
- 2 ，用加粗斜体标识，nums2 = [1,2,3,4]。下一个更大元素是 3 。
- 4 ，用加粗斜体标识，nums2 = [1,2,3,4]。不存在下一个更大元素，所以答案是 -1 。
```

 

**提示：**

- `1 <= nums1.length <= nums2.length <= 1000`
- `0 <= nums1[i], nums2[i] <= 104`
- `nums1`和`nums2`中所有整数 **互不相同**
- `nums1` 中的所有整数同样出现在 `nums2` 中

 

**进阶：**你可以设计一个时间复杂度为 `O(nums1.length + nums2.length)` 的解决方案吗？

## 代码

```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {

        // 结果数组
        int[] result = new int[nums1.length];

        // 单调栈：直接存储元素值
        // 栈内元素保持单调递减
        Deque<Integer> st = new ArrayDeque<>();

        // key：某个数字
        // value：该数字右侧第一个比它大的数字
        Map<Integer, Integer> map = new HashMap<>();

        // 遍历 nums2，求每个数字的下一个更大元素
        for (int num : nums2) {

            // 当前数字比栈顶大
            // 说明找到了栈顶元素的下一个更大元素
            while (!st.isEmpty() && num > st.peek()) {

                // 记录映射关系：
                // 栈顶元素 -> 当前更大的元素
                map.put(st.pop(), num);
            }

            // 当前元素入栈
            // 等待未来出现更大的元素
            st.push(num);
        }

        // 根据 nums1 查询答案
        for (int i = 0; i < nums1.length; i++) {

            // 如果存在更大元素则返回
            // 否则返回 -1
            result[i] = map.getOrDefault(nums1[i], -1);
        }

        return result;
    }
}
```

# 503.下一个更大元素II

## 题目描述

给定一个循环数组 `nums` （ `nums[nums.length - 1]` 的下一个元素是 `nums[0]` ），返回 *`nums` 中每个元素的 **下一个更大元素*** 。

数字 `x` 的 **下一个更大的元素** 是按数组遍历顺序，这个数字之后的第一个比它更大的数，这意味着你应该循环地搜索它的下一个更大的数。如果不存在，则输出 `-1` 。

 

**示例 1:**

```
输入: nums = [1,2,1]
输出: [2,-1,2]
解释: 第一个 1 的下一个更大的数是 2；
数字 2 找不到下一个更大的数； 
第二个 1 的下一个最大的数需要循环搜索，结果也是 2。
```

**示例 2:**

```
输入: nums = [1,2,3,4,3]
输出: [2,3,4,-1,4]
```

 

**提示:**

- `1 <= nums.length <= 104`
- `-109 <= nums[i] <= 109`

## 代码

```java
class Solution {
    public int[] nextGreaterElements(int[] nums) {

        int n = nums.length;

        // 默认答案全部设为 -1
        int[] result = new int[n];
        Arrays.fill(result, -1);

        // 单调递减栈，存储下标
        Deque<Integer> st = new ArrayDeque<>();

        // 遍历两轮，模拟循环数组
        for (int round = 0; round < 2; round++) {

            for (int i = 0; i < n; i++) {

                // 当前元素比栈顶对应元素大
                // 说明找到了栈顶元素的下一个更大元素
                while (!st.isEmpty() && nums[i] > nums[st.peek()]) {

                    // 只给还没找到答案的位置赋值
                    if (result[st.peek()] == -1) {
                        result[st.peek()] = nums[i];
                    }

                    st.pop();
                }

                // 当前下标入栈
                st.push(i);
            }
        }

        return result;
    }
}
```

# 42.接雨水

## 题目描述

给定 `n` 个非负整数表示每个宽度为 `1` 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

 

**示例 1：**

![img](./LeetCode--代码随想录(单调栈).assets/rainwatertrap.png)

```
输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]
输出：6
解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 
```

**示例 2：**

```
输入：height = [4,2,0,3,2,5]
输出：9
```

 

**提示：**

- `n == height.length`
- `1 <= n <= 2 * 104`
- `0 <= height[i] <= 105`

## 图解思路

![image-20260606142807886](./LeetCode--代码随想录(单调栈).assets/image-20260606142807886.png)	

## 代码

```java
class Solution {
    public int trap(int[] height) {

        // 单调栈：存储柱子的下标
        // 栈内对应高度保持单调递减
        Deque<Integer> stack = new ArrayDeque<>();

        // 最终接到的雨水总量
        int result = 0;

        for (int i = 0; i < height.length; i++) {

            // 当前柱子比栈顶柱子高
            // 说明形成了一个"凹槽"，可以计算雨水
            while (!stack.isEmpty() && height[stack.peek()] < height[i]) {

                // 凹槽底部高度
                int mid = height[stack.peek()];
                stack.pop();

                // 弹出后如果栈为空
                // 说明左边没有挡板，无法接水
                if (!stack.isEmpty()) {

                    // 左挡板高度
                    int left = height[stack.peek()];

                    // 水面高度 = 左右挡板较矮者
                    int waterHeight = Math.min(height[i], left) - mid;

                    // 水槽宽度
                    // 当前下标 i 是右挡板
                    // stack.peek() 是左挡板
                    int width = i - stack.peek() - 1;

                    // 累加当前凹槽的雨水量
                    result += waterHeight * width;
                }
            }

            // 当前柱子入栈
            stack.push(i);
        }

        return result;
    }
}
```



# 84.柱状图中最大的矩形

## 题目描述

给定 *n* 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。

求在该柱状图中，能够勾勒出来的矩形的最大面积。

 

**示例 1:**

![img](./LeetCode--代码随想录(单调栈).assets/histogram.jpg)

```
输入：heights = [2,1,5,6,2,3]
输出：10
解释：最大的矩形为图中红色区域，面积为 10
```

**示例 2：**

![img](./LeetCode--代码随想录(单调栈).assets/histogram-1.jpg)

```
输入： heights = [2,4]
输出： 4
```

 

**提示：**

- `1 <= heights.length <=105`
- `0 <= heights[i] <= 104`

## 图解思路

![image-20260606161459616](./LeetCode--代码随想录(单调栈).assets/image-20260606161459616.png)

## 代码

```java
class Solution {
    public int largestRectangleArea(int[] heights) {

        // 记录最大矩形面积
        int result = 0;

        // 单调栈：存储柱子下标
        // 栈内对应高度保持单调递增
        Deque<Integer> stack = new ArrayDeque<>();

        // 在原数组左右各补一个高度为 0 的柱子
        // 作用：
        // 1. 左边的 0 防止计算宽度时栈为空
        // 2. 右边的 0 强制弹出所有剩余柱子
        int[] newHeights = new int[heights.length + 2];

        for (int i = 1; i < newHeights.length - 1; i++) {
            newHeights[i] = heights[i - 1];
        }

        // 遍历新数组
        for (int i = 0; i < newHeights.length; i++) {

            // 当前柱子更矮
            // 说明以栈顶柱子为高度的最大矩形已经确定
            while (!stack.isEmpty()
                    && newHeights[i] < newHeights[stack.peek()]) {

                // 当前被弹出的柱子高度
                int h = newHeights[stack.pop()];

                // 此时：
                // i 是右边第一个比 h 小的位置
                // stack.peek() 是左边第一个比 h 小的位置

                // 矩形宽度
                int width = i - stack.peek() - 1;

                // 更新最大面积
                result = Math.max(result, width * h);
            }

            // 当前下标入栈
            stack.push(i);
        }

        return result;
    }
}
```

