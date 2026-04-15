# 232.用栈实现队列

## 题目描述

请你仅使用两个栈实现先入先出队列。队列应当支持一般队列支持的所有操作（`push`、`pop`、`peek`、`empty`）：

实现 `MyQueue` 类：

- `void push(int x)` 将元素 x 推到队列的末尾
- `int pop()` 从队列的开头移除并返回元素
- `int peek()` 返回队列开头的元素
- `boolean empty()` 如果队列为空，返回 `true` ；否则，返回 `false`

**说明：**

- 你 **只能** 使用标准的栈操作 —— 也就是只有 `push to top`, `peek/pop from top`, `size`, 和 `is empty` 操作是合法的。
- 你所使用的语言也许不支持栈。你可以使用 list 或者 deque（双端队列）来模拟一个栈，只要是标准的栈操作即可。

 

**示例 1：**

```
输入：
["MyQueue", "push", "push", "peek", "pop", "empty"]
[[], [1], [2], [], [], []]
输出：
[null, null, null, 1, 1, false]

解释：
MyQueue myQueue = new MyQueue();
myQueue.push(1); // queue is: [1]
myQueue.push(2); // queue is: [1, 2] (leftmost is front of the queue)
myQueue.peek(); // return 1
myQueue.pop(); // return 1, queue is [2]
myQueue.empty(); // return false
```

 

**提示：**

- `1 <= x <= 9`
- 最多调用 `100` 次 `push`、`pop`、`peek` 和 `empty`
- 假设所有操作都是有效的 （例如，一个空的队列不会调用 `pop` 或者 `peek` 操作）

 

**进阶：**

- 你能否实现每个操作均摊时间复杂度为 `O(1)` 的队列？换句话说，执行 `n` 个操作的总时间复杂度为 `O(n)` ，即使其中一个操作可能花费较长时间。

## 解题思路

![image-20260413120441122](./LeetCode--代码随想录(栈与队列).assets/image-20260413120441122.png)

![image-20260413120355673](./LeetCode--代码随想录(栈与队列).assets/image-20260413120355673.png)

## 代码

```java
class MyQueue {

    private Stack<Integer> stackIn;
    private Stack<Integer> stackOut;
    

    public MyQueue() {
        // 初始化入队栈/出队栈
        stackIn = new Stack<>();
        stackOut = new Stack<>();
    }
    
    public void push(int x) {
        // 入队全部压入入队栈
        stackIn.push(x);
    }
    
    public int pop() {
        // 出队
        // 检查出队栈是否为空
        if(stackOut.isEmpty()){
            // 为空则清空入队栈
            while(!stackIn.isEmpty()){
                stackOut.push(stackIn.peek());
                stackIn.pop();
            }
        }
        // 给出队栈栈顶元素出栈
        int result = stackOut.peek();
        stackOut.pop();
        return result;
    }
    
    public int peek() {
        if(stackOut.isEmpty()){
            while(!stackIn.isEmpty()){
                stackOut.push(stackIn.peek());
                stackIn.pop();
            }
        }
        return stackOut.peek();
    }
    
    public boolean empty() {
        // 若两个栈均为空则队列为空
        if(!stackIn.isEmpty()||!stackOut.isEmpty())
        return false;
        return true;
    }
}

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue obj = new MyQueue();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.peek();
 * boolean param_4 = obj.empty();
 */
```

# 225.用队列实现栈

## 题目描述

请你仅使用两个队列实现一个后入先出（LIFO）的栈，并支持普通栈的全部四种操作（`push`、`top`、`pop` 和 `empty`）。

实现 `MyStack` 类：

- `void push(int x)` 将元素 x 压入栈顶。
- `int pop()` 移除并返回栈顶元素。
- `int top()` 返回栈顶元素。
- `boolean empty()` 如果栈是空的，返回 `true` ；否则，返回 `false` 。

 

**注意：**

- 你只能使用队列的标准操作 —— 也就是 `push to back`、`peek/pop from front`、`size` 和 `is empty` 这些操作。
- 你所使用的语言也许不支持队列。 你可以使用 list （列表）或者 deque（双端队列）来模拟一个队列 , 只要是标准的队列操作即可。

 

**示例：**

```
输入：
["MyStack", "push", "push", "top", "pop", "empty"]
[[], [1], [2], [], [], []]
输出：
[null, null, null, 2, 2, false]

解释：
MyStack myStack = new MyStack();
myStack.push(1);
myStack.push(2);
myStack.top(); // 返回 2
myStack.pop(); // 返回 2
myStack.empty(); // 返回 False
```

 

**提示：**

- `1 <= x <= 9`
- 最多调用`100` 次 `push`、`pop`、`top` 和 `empty`
- 每次调用 `pop` 和 `top` 都保证栈不为空

 

**进阶：**你能否仅用一个队列来实现栈。

## 解题思路

![image-20260413125045148](./LeetCode--代码随想录(栈与队列).assets/image-20260413125045148.png)

## 代码

```java
class MyStack {

    private Queue<Integer> que;

    public MyStack() {
        // 初始化队列
        que = new ArrayDeque<>();
    }
    
    public void push(int x) {
        que.add(x);
    }
    
    public int pop() {
        int n = que.size();
        // 弹出前n-1个元素 再重新入队
        while(n-- > 1){
            int ele = que.poll();
            que.add(ele);
        }
        // 此时给最后一个元素出队
        return que.poll();
    }
    
    public int top() {
        int n = que.size();
        // 弹出前n-1个元素 再重新入队
        while(n-- > 1){
            int ele = que.poll();
            que.add(ele);
        }
        int result = que.peek();
        que.poll();
        que.add(result);
        // 此时返回最后一个元素
        return result;
    }
    
    public boolean empty() {
        return que.isEmpty();
    }
}

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack obj = new MyStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.top();
 * boolean param_4 = obj.empty();
 */
```

# 20.有效的括号

## 题目描述

给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串 `s` ，判断字符串是否有效。

有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。
2. 左括号必须以正确的顺序闭合。
3. 每个右括号都有一个对应的相同类型的左括号。

 

**示例 1：**

**输入：**s = "()"

**输出：**true

**示例 2：**

**输入：**s = "()[]{}"

**输出：**true

**示例 3：**

**输入：**s = "(]"

**输出：**false

**示例 4：**

**输入：**s = "([])"

**输出：**true

**示例 5：**

**输入：**s = "([)]"

**输出：**false

 

**提示：**

- `1 <= s.length <= 104`
- `s` 仅由括号 `'()[]{}'` 组成

## 解题思路

以输入示例"([])"为例，只有输入为右括号时再进行出栈。最后检查栈是否为空！

![image-20260415111643264](./LeetCode--代码随想录(栈与队列).assets/image-20260415111643264.png)

## 代码

```java
class Solution {
    public boolean isValid(String s) {
        // 创建一个栈用于存放括号数组
        Deque<Character> stack = new LinkedList<>();
        // 将字符串转换为字符数组处理
        char[] array = s.toCharArray();
        int i = 0;
        // 遍历字符数组
        while(i<array.length){
            if(array[i] == ')'){
                // 如果 () 匹配则出栈
                if(!stack.isEmpty() && stack.peek()=='('){
                    stack.pop();
                } else{
                    return false;
                }
            } else if(array[i] == '}'){
                // 如果 {} 匹配则出栈
                if(!stack.isEmpty() && stack.peek()=='{'){
                    stack.pop();
                } else{
                    return false;
                }
            }else if(array[i] == ']'){
                // 如果 [] 匹配则出栈
                if(!stack.isEmpty() && stack.peek()=='['){
                    stack.pop();
                } else{
                    return false;
                }
            } else{
                // 都不匹配就入栈
                stack.push(array[i]);
            }
            i++;
        }

        // 若栈为空则返回true
        return stack.isEmpty();
    }
}
```

# 1047. 删除字符串中的所有相邻重复项

## 题目描述

给出由小写字母组成的字符串 `s`，**重复项删除操作**会选择两个相邻且相同的字母，并删除它们。

在 `s` 上反复执行重复项删除操作，直到无法继续删除。

在完成所有重复项删除操作后返回最终的字符串。答案保证唯一。

 

**示例：**

```
输入："abbaca"
输出："ca"
解释：
例如，在 "abbaca" 中，我们可以删除 "bb" 由于两字母相邻且相同，这是此时唯一可以执行删除操作的重复项。之后我们得到字符串 "aaca"，其中又只有 "aa" 可以执行重复项删除操作，所以最后的字符串为 "ca"。
```

 

**提示：**

1. `1 <= s.length <= 105`
2. `s` 仅由小写英文字母组成。

## 解题思路

与20.有效的括号类似，匹配即将进栈的元素与栈顶元素即可。

![image-20260415143608916](./LeetCode--代码随想录(栈与队列).assets/image-20260415143608916.png)

## 代码

```java
class Solution {
    public String removeDuplicates(String s) {
        // 转换成字符数组
        char[] array = s.toCharArray();
        // 使用双端队列存储
        Deque<Character> deque = new LinkedList<>();

        for(char ch : array){
            // 队列非空时检查栈顶元素
            if(!deque.isEmpty() && ch == deque.peek()){
                // 匹配则出栈
                deque.pop();
            } else{
                // 未匹配则压栈
                deque.push(ch);
            }
        }
        // 用新字符数组输出
        char[] result = new char[deque.size()];
        for(int i=result.length-1; i>=0; i--){
            result[i] = deque.peek();
            deque.pop();
        }
        return new String(result);

    }
}
```

# 150.逆波兰表达式求值

## 题目描述

给你一个字符串数组 `tokens` ，表示一个根据 [逆波兰表示法](https://baike.baidu.com/item/逆波兰式/128437) 表示的算术表达式。

请你计算该表达式。返回一个表示表达式值的整数。

**注意：**

- 有效的算符为 `'+'`、`'-'`、`'*'` 和 `'/'` 。
- 每个操作数（运算对象）都可以是一个整数或者另一个表达式。
- 两个整数之间的除法总是 **向零截断** 。
- 表达式中不含除零运算。
- 输入是一个根据逆波兰表示法表示的算术表达式。
- 答案及所有中间计算结果可以用 **32 位** 整数表示。

 

**示例 1：**

```
输入：tokens = ["2","1","+","3","*"]
输出：9
解释：该算式转化为常见的中缀算术表达式为：((2 + 1) * 3) = 9
```

**示例 2：**

```
输入：tokens = ["4","13","5","/","+"]
输出：6
解释：该算式转化为常见的中缀算术表达式为：(4 + (13 / 5)) = 6
```

**示例 3：**

```
输入：tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
输出：22
解释：该算式转化为常见的中缀算术表达式为：
  ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22
```

 

**提示：**

- `1 <= tokens.length <= 104`
- `tokens[i]` 是一个算符（`"+"`、`"-"`、`"*"` 或 `"/"`），或是在范围 `[-200, 200]` 内的一个整数

 

**逆波兰表达式：**

逆波兰表达式是一种后缀表达式，所谓后缀就是指算符写在后面。

- 平常使用的算式则是一种中缀表达式，如 `( 1 + 2 ) * ( 3 + 4 )` 。
- 该算式的逆波兰表达式写法为 `( ( 1 2 + ) ( 3 4 + ) * )` 。

逆波兰表达式主要有以下两个优点：

- 去掉括号后表达式无歧义，上式即便写成 `1 2 + 3 4 + * `也可以依据次序计算出正确结果。
- 适合用栈操作运算：遇到数字则入栈；遇到算符则取出栈顶两个数字进行计算，并将结果压入栈中

## 解题思路

中缀表达式，适合使用栈存储来计算，如下图所示：

![image-20260415151134594](./LeetCode--代码随想录(栈与队列).assets/image-20260415151134594.png)

## 代码

```java
class Solution {
    public int evalRPN(String[] tokens) {
        // 初始化栈,存放待计算的整数
        Stack<Integer> stack = new Stack<>();
        // 遍历tokens数组
        for(String s : tokens){
            // 如果是运算符，取出两个栈顶元素进行计算
            if("+".equals(s)){
                int num1 = stack.pop();
                int num2 = stack.pop();
                stack.push(num2 + num1);
            } else if("-".equals(s)){
                int num1 = stack.pop();
                int num2 = stack.pop();
                stack.push(num2 - num1);
            } else if("*".equals(s)){
                int num1 = stack.pop();
                int num2 = stack.pop();
                stack.push(num2 * num1);
            } else if("/".equals(s)){
                int num1 = stack.pop();
                int num2 = stack.pop();
                stack.push(num2 / num1);
            } else{
                // 其余数字压栈
                stack.push(Integer.parseInt(s));
            }
        }
        return stack.pop();
    }
}
```

# 239.滑动窗口最大值

## 题目描述

给你一个整数数组 `nums`，有一个大小为 `k` 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 `k` 个数字。滑动窗口每次只向右移动一位。

返回 *滑动窗口中的最大值* 。

 

**示例 1：**

```
输入：nums = [1,3,-1,-3,5,3,6,7], k = 3
输出：[3,3,5,5,6,7]
解释：
滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

**示例 2：**

```
输入：nums = [1], k = 1
输出：[1]
```

 

**提示：**

- `1 <= nums.length <= 105`
- `-104 <= nums[i] <= 104`
- `1 <= k <= nums.length`

 

------

## 解题思路

使用**双端队列**解决，构造自己的双端队列，保持队列中**队首元素**始终最大，队列按照从大到小，单调递减排列。

![image-20260415161105843](./LeetCode--代码随想录(栈与队列).assets/image-20260415161105843.png)

![image-20260415161112381](./LeetCode--代码随想录(栈与队列).assets/image-20260415161112381.png)

## 代码

```java
class MyQueue{
    private Deque<Integer> deque;
    
    public MyQueue(){
        deque = new LinkedList<>();
    }

    // 入队操作
    public void add(int num){
        // 保证队头最大并且保证队列单调递减
        // 添加新元素时与队尾进行比较
        while(!deque.isEmpty() && num > deque.getLast()){
            deque.removeLast();
        }
        deque.add(num);
    }

    // 出队操作
    public void poll(int num){
        // 若出队时原数组中元素在队列中，则出队（一定为peek）
        // 否则，已经出过队不用管
        if(!deque.isEmpty() && num == deque.peek()){
            deque.poll();
        }
    }
    
    // 返回队头
    public int peek(){
        return deque.peek();
    }

}

class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        // 边界条件判断
        if(nums.length == 1 || k == 1){
            return nums;
        }
        // 创建返回数组
        int[] result = new int[nums.length - k + 1];
        MyQueue que = new MyQueue();
        for(int i=0; i<k; i++){
            // 为最早的一组入队
            que.add(nums[i]);
        }
        result[0] = que.peek();
        // 滑动窗口
        for(int i=k; i<nums.length; i++){
            // 先出队,再入队
            que.poll(nums[i-k]);
            que.add(nums[i]);
            result[i-k+1] = que.peek();
        }
        return result;
    }
}
```

# 347.前 K 个高频元素

## 题目描述

给你一个整数数组 `nums` 和一个整数 `k` ，请你返回其中出现频率前 `k` 高的元素。你可以按 **任意顺序** 返回答案。

 

**示例 1：**

**输入：**nums = [1,1,1,2,2,3], k = 2

**输出：**[1,2]

**示例 2：**

**输入：**nums = [1], k = 1

**输出：**[1]

**示例 3：**

**输入：**nums = [1,2,1,2,1,2,3,1,3,2], k = 2

**输出：**[1,2]

 

**提示：**

- `1 <= nums.length <= 105`
- `-104 <= nums[i] <= 104`
- `k` 的取值范围是 `[1, 数组中不相同的元素的个数]`
- 题目数据保证答案唯一，换句话说，数组中前 `k` 个高频元素的集合是唯一的

 

**进阶：**你所设计算法的时间复杂度 **必须** 优于 `O(n log n)` ，其中 `n` 是数组大小。

## 解题思路

该题借助Map，key为num，value为出现次数，来统计出现的数字次数。

借助PriorityQueue，优先级队列（大顶堆/小顶堆）的一种实现方式，来对Map中的元素进行频率排序。

大顶堆的定义：

1、大顶堆是一棵完全二叉树，**每个父节点的值都大于或等于其子节点的值**。

2、在堆结构中，**堆顶（根节点）始终是整个集合中的最大元素**。

![image-20260415203243428](./LeetCode--代码随想录(栈与队列).assets/image-20260415203243428.png)

## 代码

```java
class Solution {
    // 基于大顶堆实现
    public int[] topKFrequent(int[] nums, int k) {
        // 用Map来存放数和频率,key为数,value为频率
        Map<Integer, Integer> map = new HashMap<>();
        for(int num : nums){
            // 从1开始计数
            map.put(num,map.getOrDefault(num,0) + 1);
        }
        // 使用PriorityQueue实现大顶堆
        // Comparator接口默认返回o1-o2,在PriorityQueue中默认为小顶堆
        // 这里重写Comparator接口的compare方法(o1, o2) -> o2[1] - o1[1],频率相减
        // 变成大顶堆排序
        PriorityQueue<int[]> pq = new PriorityQueue<>((o1,o2) -> o2[1] - o1[1]);
        for(Map.Entry<Integer,Integer> entry : map.entrySet()){
            // 向PriorityQueue中插入数据
            pq.add(new int[] {entry.getKey(),entry.getValue()});
        }
        int[] result = new int[k];
        for(int i=0; i<k; i++){
            // 将前k个高频率插入返回数组
            result[i] = pq.poll()[0];
        }
        return result;
    }
}
```

