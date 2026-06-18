# 797. 所有可能的路径

## 题目描述

给你一个有 `n` 个节点的 **有向无环图（DAG）**，请你找出从节点 `0` 到节点 `n-1` 的所有路径并输出（**不要求按特定顺序**）

 `graph[i]` 是一个从节点 `i` 可以访问的所有节点的列表（即从节点 `i` 到节点 `graph[i][j]`存在一条有向边）。

 

**示例 1：**

![img](./LeetCode--代码随想录(图论).assets/all_1.jpg)

```
输入：graph = [[1,2],[3],[3],[]]
输出：[[0,1,3],[0,2,3]]
解释：有两条路径 0 -> 1 -> 3 和 0 -> 2 -> 3
```

**示例 2：**

![img](./LeetCode--代码随想录(图论).assets/all_2.jpg)

```
输入：graph = [[4,3,1],[3,2,4],[3],[4],[]]
输出：[[0,4],[0,3,4],[0,1,3,4],[0,1,2,3,4],[0,1,4]]
```

 

**提示：**

- `n == graph.length`
- `2 <= n <= 15`
- `0 <= graph[i][j] < n`
- `graph[i][j] != i`（即不存在自环）
- `graph[i]` 中的所有元素 **互不相同**
- 保证输入为 **有向无环图（DAG）**

## 代码

力扣：

```java
class Solution {

    // 当前搜索路径
    // 例如：0 -> 1 -> 3
    // path = [0, 1, 3]
    List<Integer> path = new ArrayList<>();

    // 存放所有满足条件的路径
    List<List<Integer>> result = new ArrayList<>();

    // 记录节点是否已经在当前路径中出现
    boolean[] visited;

    public void dfs(int[][] graph) {

        // 到达终点 n-1
        if (path.get(path.size() - 1) == graph.length - 1) {

            // 当前路径加入答案
            // 需要拷贝一份，否则后续回溯会修改原路径
            result.add(new ArrayList<>(path));

            return;
        }

        // 当前所在节点
        int cur = path.get(path.size() - 1);

        // 遍历当前节点的所有邻接节点
        for (int i = 0; i < graph[cur].length; i++) {

            int next = graph[cur][i];

            // 该节点未被访问过
            if (!visited[next]) {

                // 做选择
                path.add(next);
                visited[next] = true;

                // 继续向下搜索
                dfs(graph);

                // 回溯
                visited[next] = false;
                path.remove(path.size() - 1);
            }
        }
    }

    public List<List<Integer>> allPathsSourceTarget(int[][] graph) {

        // graph.length = 节点个数
        visited = new boolean[graph.length];

        // 起点固定为0
        path.add(0);
        visited[0] = true;

        // DFS搜索所有路径
        dfs(graph);

        return result;
    }
}
```

标准输入输出模式：

```java
import java.util.*;

public class Main {

    // 邻接矩阵
    // matrix[i][j] = 1 表示存在一条 i -> j 的有向边
    public static int[][] matrix;

    // 记录当前搜索路径
    // 例如：1 -> 2 -> 5
    // path = [1, 2, 5]
    public static List<Integer> path = new LinkedList<>();

    // 记录从节点1到节点N的路径数量
    // 用于判断最终是否存在可达路径
    public static int result = 0;

    public static void dfs(int[][] matrix) {

        // 如果当前已经到达终点N
        if (path.get(path.size() - 1) == matrix.length - 1) {

            result++;

            // 输出当前路径
            for (int i = 0; i < path.size() - 1; i++) {
                System.out.print(path.get(i) + " ");
            }
            System.out.println(path.get(path.size() - 1));

            return;
        }

        // 当前所在节点
        int cur = path.get(path.size() - 1);

        // 枚举当前节点能够到达的所有下一节点
        for (int j = 1; j < matrix.length; j++) {

            // 存在边 cur -> j
            // 并且 j 尚未出现在当前路径中
            // 防止在有环图中出现死循环
            if (matrix[cur][j] == 1 && !path.contains(j)) {

                // 做选择：访问节点j
                path.add(j);

                // 继续向下搜索
                dfs(matrix);

                // 回溯：撤销本次选择
                path.remove(path.size() - 1);
            }
        }

        return;
    }

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        // 节点数量N
        int N = sc.nextInt();

        // 边数量M
        int M = sc.nextInt();

        // 创建邻接矩阵
        // 节点编号从1开始，因此开 N+1
        matrix = new int[N + 1][N + 1];

        // 读入所有有向边
        for (int i = 0; i < M; i++) {
            int begin = sc.nextInt();
            int end = sc.nextInt();

            matrix[begin][end] = 1;
        }

        // 起点固定为1
        path.add(1);

        // DFS搜索所有从1到N的路径
        dfs(matrix);

        // 如果一条路径都没找到
        if (result == 0) {
            System.out.println(-1);
        }

        sc.close();
    }
}
```

#  200. 岛屿数量

## 题目描述

给你一个由 `'1'`（陆地）和 `'0'`（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。

 

**示例 1：**

```
输入：grid = [
  ['1','1','1','1','0'],
  ['1','1','0','1','0'],
  ['1','1','0','0','0'],
  ['0','0','0','0','0']
]
输出：1
```

**示例 2：**

```
输入：grid = [
  ['1','1','0','0','0'],
  ['1','1','0','0','0'],
  ['0','0','1','0','0'],
  ['0','0','0','1','1']
]
输出：3
```

 

**提示：**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 300`
- `grid[i][j]` 的值为 `'0'` 或 `'1'`

## 代码

深度优先搜索（ACM模式）

```java
import java.util.*;

public class Main {

    // 记录岛屿数量
    public static int result = 0;

    // 标记某个位置是否已经访问过
    public static boolean[][] visited;

    // 地图行数和列数
    public static int M;
    public static int N;

    // 四个方向：左、右、下、上
    public static int[][] dir = {
        {0, -1},
        {0, 1},
        {1, 0},
        {-1, 0}
    };

    public static void dfs(int[][] grid, int x, int y) {

        // 越界
        // 已访问
        // 海洋(0)
        // 三种情况直接返回
        if (x < 0 || x >= N ||
            y < 0 || y >= M ||
            visited[x][y] ||
            grid[x][y] == 0) {
            return;
        }

        // 标记当前位置已访问
        visited[x][y] = true;

        // 向四个方向继续扩散
        for (int i = 0; i < 4; i++) {

            int newX = x + dir[i][0];
            int newY = y + dir[i][1];

            dfs(grid, newX, newY);
        }
    }

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        // N行 M列
        N = sc.nextInt();
        M = sc.nextInt();

        int[][] grid = new int[N][M];

        // 访问标记数组
        visited = new boolean[N][M];

        // 读入地图
        // 1表示陆地
        // 0表示海洋
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < M; j++) {
                grid[i][j] = sc.nextInt();
            }
        }

        // 遍历整张地图
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < M; j++) {

                // 找到一块未访问过的陆地
                if (grid[i][j] == 1 && !visited[i][j]) {

                    // 发现一个新的岛屿
                    result++;

                    // 将整个岛屿全部标记为已访问
                    dfs(grid, i, j);
                }
            }
        }

        // 输出岛屿数量
        System.out.println(result);

        sc.close();
    }
}
```



深度优先搜索（核心代码模式）

```java
class Solution {

    // 访问标记数组
    boolean[][] visited;

    // 四个方向：左、右、下、上
    int[][] dir = {
        {0, -1},
        {0, 1},
        {1, 0},
        {-1, 0}
    };

    // 地图行数和列数
    int N;
    int M;

    public void dfs(char[][] grid, int x, int y) {

        // 当前陆地标记为已访问
        visited[x][y] = true;

        // 向四个方向扩散
        for (int i = 0; i < 4; i++) {

            int newX = x + dir[i][0];
            int newY = y + dir[i][1];

            // 越界
            // 已访问
            // 海洋
            // 三种情况直接跳过
            if (newX < 0 || newY < 0 ||
                newX >= N || newY >= M ||
                visited[newX][newY] ||
                grid[newX][newY] == '0') {
                continue;
            }

            // 继续搜索相邻陆地
            dfs(grid, newX, newY);
        }
    }

    public int numIslands(char[][] grid) {

        // 岛屿数量
        int result = 0;

        N = grid.length;
        M = grid[0].length;

        visited = new boolean[N][M];

        // 遍历整张地图
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < M; j++) {

                // 找到一块未访问过的陆地
                if (grid[i][j] == '1' && !visited[i][j]) {

                    // 发现新的岛屿
                    result++;

                    // DFS淹没整座岛屿
                    dfs(grid, i, j);
                }
            }
        }

        return result;
    }
}
```



广度优先搜索（ACM模式）

```java
import java.util.*;

public class Main {

    // 岛屿数量
    public static int result = 0;

    // 访问标记数组
    public static boolean[][] visited;

    // 地图行数和列数
    public static int M;
    public static int N;

    // 四个方向：左、右、下、上
    public static int[][] dir = {
        {0, -1},
        {0, 1},
        {1, 0},
        {-1, 0}
    };

    public static void bfs(int[][] grid, int x, int y) {

        // BFS队列
        Queue<Pair> que = new ArrayDeque<>();

        // 起点入队
        que.add(new Pair(x, y));

        // 标记起点已访问
        visited[x][y] = true;

        while (!que.isEmpty()) {

            // 取出当前节点
            x = que.peek().first;
            y = que.poll().second;

            // 向四个方向扩散
            for (int i = 0; i < 4; i++) {

                int nextX = x + dir[i][0];
                int nextY = y + dir[i][1];

                // 越界直接跳过
                if (nextX < 0 || nextX >= N ||
                    nextY < 0 || nextY >= M) {
                    continue;
                }

                // 找到未访问过的陆地
                if (grid[nextX][nextY] == 1 &&
                    !visited[nextX][nextY]) {

                    // 入队等待后续搜索
                    que.add(new Pair(nextX, nextY));

                    // 标记已访问
                    visited[nextX][nextY] = true;
                }
            }
        }
    }

    // 记录网格中的一个坐标点
    public static class Pair {

        int first;
        int second;

        Pair(int first, int second) {
            this.first = first;
            this.second = second;
        }
    }

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        // N行 M列
        N = sc.nextInt();
        M = sc.nextInt();

        int[][] grid = new int[N][M];

        // 访问标记数组
        visited = new boolean[N][M];

        // 读入地图
        // 1表示陆地
        // 0表示海洋
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < M; j++) {
                grid[i][j] = sc.nextInt();
            }
        }

        // 遍历整张地图
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < M; j++) {

                // 找到一块未访问过的陆地
                if (grid[i][j] == 1 &&
                    !visited[i][j]) {

                    // 发现新的岛屿
                    result++;

                    // BFS遍历整座岛屿
                    // 将同一岛屿上的所有陆地标记为已访问
                    bfs(grid, i, j);
                }
            }
        }

        // 输出岛屿数量
        System.out.println(result);

        sc.close();
    }
}
```



广度优先搜索（核心代码模式）

```java
class Solution {

    // 访问标记数组
    boolean[][] visited;

    // 四个方向：左、右、下、上
    int[][] dir = {
        {0, -1},
        {0, 1},
        {1, 0},
        {-1, 0}
    };

    // 地图行数和列数
    int N;
    int M;

    public void bfs(char[][] grid, int x, int y) {

        // BFS队列
        Queue<Pair> que = new ArrayDeque<>();

        // 起点入队
        que.add(new Pair(x, y));

        // 标记起点已访问
        visited[x][y] = true;

        while (!que.isEmpty()) {

            // 取出当前节点
            x = que.peek().first;
            y = que.poll().second;

            // 向四个方向扩展
            for (int i = 0; i < 4; i++) {

                int nextX = x + dir[i][0];
                int nextY = y + dir[i][1];

                // 越界直接跳过
                if (nextX < 0 || nextX >= N ||
                    nextY < 0 || nextY >= M) {
                    continue;
                }

                // 找到未访问过的陆地
                if (grid[nextX][nextY] == '1'
                    && !visited[nextX][nextY]) {

                    // 入队等待后续搜索
                    que.add(new Pair(nextX, nextY));

                    // 标记已访问
                    visited[nextX][nextY] = true;
                }
            }
        }
    }

    // 存储二维坐标
    public class Pair {

        int first;
        int second;

        Pair(int first, int second) {
            this.first = first;
            this.second = second;
        }
    }

    public int numIslands(char[][] grid) {

        // 岛屿数量
        int result = 0;

        N = grid.length;
        M = grid[0].length;

        visited = new boolean[N][M];

        // 遍历整张地图
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < M; j++) {

                // 找到一块未访问过的陆地
                if (grid[i][j] == '1' && !visited[i][j]) {

                    // 发现新的岛屿
                    result++;

                    // BFS遍历整座岛屿
                    // 将同一岛屿上的所有陆地标记为已访问
                    bfs(grid, i, j);
                }
            }
        }

        return result;
    }
}
```

# 695.岛屿的最大面积

## 题目描述

给你一个大小为 `m x n` 的二进制矩阵 `grid` 。

**岛屿** 是由一些相邻的 `1` (代表土地) 构成的组合，这里的「相邻」要求两个 `1` 必须在 **水平或者竖直的四个方向上** 相邻。你可以假设 `grid` 的四个边缘都被 `0`（代表水）包围着。

岛屿的面积是岛上值为 `1` 的单元格的数目。

计算并返回 `grid` 中最大的岛屿面积。如果没有岛屿，则返回面积为 `0` 。

 

**示例 1：**

![img](./LeetCode--代码随想录(图论).assets/maxarea1-grid.jpg)

```
输入：grid = [[0,0,1,0,0,0,0,1,0,0,0,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,1,1,0,1,0,0,0,0,0,0,0,0],[0,1,0,0,1,1,0,0,1,0,1,0,0],[0,1,0,0,1,1,0,0,1,1,1,0,0],[0,0,0,0,0,0,0,0,0,0,1,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,0,0,0,0,0,0,1,1,0,0,0,0]]
输出：6
解释：答案不应该是 11 ，因为岛屿只能包含水平或垂直这四个方向上的 1 。
```

**示例 2：**

```
输入：grid = [[0,0,0,0,0,0,0,0]]
输出：0
```

 

**提示：**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 50`
- `grid[i][j]` 为 `0` 或 `1`

## 代码

深度优先搜索（核心代码模式）：

```java
class Solution {

    // 地图行数和列数
    int N;
    int M;

    // 四个方向：右、左、上、下
    int[][] dir = {
        {0, 1},
        {0, -1},
        {-1, 0},
        {1, 0}
    };

    // 访问标记数组
    boolean[][] visited;

    // 当前岛屿面积
    int count = 0;

    public void dfs(int[][] grid, int x, int y) {

        // 向四个方向扩散
        for (int i = 0; i < 4; i++) {

            int nextX = x + dir[i][0];
            int nextY = y + dir[i][1];

            // 越界
            // 已访问
            // 海洋
            // 直接跳过
            if (nextX < 0 || nextX >= N ||
                nextY < 0 || nextY >= M ||
                visited[nextX][nextY] ||
                grid[nextX][nextY] == 0) {
                continue;
            }

            // 标记当前陆地已访问
            visited[nextX][nextY] = true;

            // 当前岛屿面积+1
            count++;

            // 继续搜索相邻陆地
            dfs(grid, nextX, nextY);
        }
    }

    public int maxAreaOfIsland(int[][] grid) {

        N = grid.length;
        M = grid[0].length;

        // 最大岛屿面积
        int result = 0;

        visited = new boolean[N][M];

        // 遍历整张地图
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < M; j++) {

                // 找到一块未访问过的陆地
                if (grid[i][j] == 1 &&
                    !visited[i][j]) {

                    // 标记起点已访问
                    visited[i][j] = true;

                    // 当前岛屿面积从1开始
                    count++;

                    // DFS统计整座岛屿面积
                    dfs(grid, i, j);

                    // 更新最大岛屿面积
                    result = Math.max(result, count);

                    // 为下一座岛屿重新计数
                    count = 0;
                }
            }
        }

        return result;
    }
}
```

# 101.孤岛的总面积

## 题目描述

给定一个由 1（陆地）和 0（水）组成的矩阵，岛屿指的是由水平或垂直方向上相邻的陆地单元格组成的区域，且完全被陆地单元格包围。孤岛是那些位于矩阵内部、所有单元格都不接触边缘的岛屿。



现在你需要计算所有孤岛的总面积，岛屿面积的计算方式为组成岛屿的陆地的总数。

输入描述

第一行包含两个整数 N, M，表示矩阵的行数和列数。之后 N 行，每行包含 M 个数字，数字为 1 或者 0。

输出描述

输出一个整数，表示所有孤岛的总面积，如果不存在孤岛，则输出 0。

输入示例

```
4 5
1 1 0 0 0
1 1 0 0 0
0 0 1 0 0
0 0 0 1 1
```

输出示例

```
1
```

提示信息

![img](./LeetCode--代码随想录(图论).assets/20240412113711_58587.png)



在矩阵中心部分的岛屿，因为没有任何一个单元格接触到矩阵边缘，所以该岛屿属于孤岛，总面积为 1。



数据范围：

1 <= M, N <= 50。

## 代码

```java
import java.util.*;

public class Main{
    
    public static int N;
    public static int M;
    
    public static int[][] dir = {{1,0},{0,1},{0,-1},{-1,0}};
	
    // 把边缘岛屿全部变成海水
    public static void dfs(int[][] grid, int x, int y){
        if(x<0 || x>N-1 || y<0 || y>M-1 || grid[x][y]==0) return;
        grid[x][y] = 0;
        for(int i=0; i<4; i++){
            int nextX = x + dir[i][0];
            int nextY = y + dir[i][1];
            dfs(grid, nextX, nextY);
        }
    }
    
    public static void main(String[] args){

        Scanner sc = new Scanner(System.in);
        N = sc.nextInt();
        M = sc.nextInt();

        int[][] grid = new int[N][M];

        for(int i=0; i<N; i++){
            for(int j=0; j<M; j++){
                grid[i][j] = sc.nextInt();
            }
        }

        for(int i=0; i<N; i++){
            if(grid[i][0]==1) dfs(grid, i, 0);
            if(grid[i][M-1]==1) dfs(grid, i, M-1);
        }


        for(int j=0; j<M; j++){
            if(grid[0][j]==1) dfs(grid, 0, j);
            if(grid[N-1][j]==1) dfs(grid, N-1, j);
        }

        int result = 0;

        for(int i=0; i<N; i++){
            for(int j=0; j<M; j++){
                if(grid[i][j] == 1){
                    result++;
                }
            }
        }

        System.out.println(result);
        sc.close();

    }
}
```

# 102.沉默孤岛

## 题目描述

给定一个由 1（陆地）和 0（水）组成的矩阵，岛屿指的是由水平或垂直方向上相邻的陆地单元格组成的区域，且完全被水域单元格包围。孤岛是那些位于矩阵内部、所有单元格都不接触边缘的岛屿。



现在你需要将所有孤岛“沉没”，即将孤岛中的所有陆地单元格（1）转变为水域单元格（0）。

输入描述

第一行包含两个整数 N, M，表示矩阵的行数和列数。

之后 N 行，每行包含 M 个数字，数字为 1 或者 0，表示岛屿的单元格。

输出描述

输出将孤岛“沉没”之后的岛屿矩阵。 注意：每个元素后面都有一个空格

输入示例

```
4 5
1 1 0 0 0
1 1 0 0 0
0 0 1 0 0
0 0 0 1 1
```

输出示例

```
1 1 0 0 0
1 1 0 0 0
0 0 0 0 0
0 0 0 1 1
```

提示信息

![img](./LeetCode--代码随想录(图论).assets/20240412144356_69900.png)



将孤岛沉没。



![img](./LeetCode--代码随想录(图论).assets/20240412144445_89755.png)



数据范围：

1 <= M, N <= 50。

## 代码

```java
import java.util.*;

public class Main{

    public static int N;
    public static int M;

    public static boolean[][] visited;
    public static int[][] dir = {{1,0},{-1,0},{0,-1},{0,1}};

    public static void dfs(int[][] grid, int x, int y){
        if(x<0 || y<0 || x>=N || y>=M || grid[x][y]==0 || visited[x][y]) return;
        visited[x][y] = true;

        for(int i=0; i<4; i++){
            int nextX = x + dir[i][0];
            int nextY = y + dir[i][1];
            dfs(grid, nextX, nextY);
        }
    }

    public static void main(String[] args){

        Scanner sc = new Scanner(System.in);
        N = sc.nextInt();
        M = sc.nextInt();
        
        int[][] grid = new int[N][M];
        visited = new boolean[N][M];

        for(int i=0; i<N; i++){
            for(int j=0; j<M; j++){
                grid[i][j] = sc.nextInt();
            }
        }

        for(int i=0; i<N; i++){
            if(grid[i][0] == 1) dfs(grid, i, 0);
            if(grid[i][M-1] == 1) dfs(grid, i, M-1);
        }

        for(int j=0; j<M; j++){
            if(grid[0][j] == 1) dfs(grid, 0, j);
            if(grid[N-1][j] == 1) dfs(grid, N-1, j);
        }

        for(int i=0; i<N; i++){
            for(int j=0; j<M-1; j++){
                if(visited[i][j]) System.out.print("1 ");
                else System.out.print("0 ");
            }
            if(visited[i][M-1]) System.out.println("1 ");
            else System.out.println("0 ");
        }

        sc.close();

    }
}
```

# 417.太平洋大西洋水流问题

## 题目描述

有一个 `m × n` 的矩形岛屿，与 **太平洋** 和 **大西洋** 相邻。 **“太平洋”** 处于大陆的左边界和上边界，而 **“大西洋”** 处于大陆的右边界和下边界。

这个岛被分割成一个由若干方形单元格组成的网格。给定一个 `m x n` 的整数矩阵 `heights` ， `heights[r][c]` 表示坐标 `(r, c)` 上单元格 **高于海平面的高度** 。

岛上雨水较多，如果相邻单元格的高度 **小于或等于** 当前单元格的高度，雨水可以直接向北、南、东、西流向相邻单元格。水可以从海洋附近的任何单元格流入海洋。

返回网格坐标 `result` 的 **2D 列表** ，其中 `result[i] = [ri, ci]` 表示雨水从单元格 `(ri, ci)` 流动 **既可流向太平洋也可流向大西洋** 。

 

**示例 1：**

![img](./LeetCode--代码随想录(图论).assets/waterflow-grid.jpg)

```
输入: heights = [[1,2,2,3,5],[3,2,3,4,4],[2,4,5,3,1],[6,7,1,4,5],[5,1,1,2,4]]
输出: [[0,4],[1,3],[1,4],[2,2],[3,0],[3,1],[4,0]]
```

**示例 2：**

```
输入: heights = [[2,1],[1,2]]
输出: [[0,0],[0,1],[1,0],[1,1]]
```

 

**提示：**

- `m == heights.length`
- `n == heights[r].length`
- `1 <= m, n <= 200`
- `0 <= heights[r][c] <= 105`

## 代码

核心代码：

```java
class Solution {

    // 存放最终答案
    // 每个元素为 [row, col]
    List<List<Integer>> result = new ArrayList<>();

    // 地图行数和列数
    int n;
    int m;

    // Pacific(太平洋)可达标记
    boolean[][] visited1;

    // Atlantic(大西洋)可达标记
    boolean[][] visited2;

    // 四个方向：下、上、右、左
    int[][] dir = {
        {1, 0},
        {-1, 0},
        {0, 1},
        {0, -1}
    };

    public void dfs(int[][] grid,
                    boolean[][] visited,
                    int x,
                    int y) {

        // 当前节点标记已访问
        visited[x][y] = true;

        // 向四个方向扩散
        for (int i = 0; i < 4; i++) {

            int nextX = x + dir[i][0];
            int nextY = y + dir[i][1];

            // 越界
            // 已访问
            // 直接跳过
            if (nextX < 0 || nextY < 0 ||
                nextX >= n || nextY >= m ||
                visited[nextX][nextY]) {
                continue;
            }

            // 反向搜索
            // 只能从低处走向高处或等高处
            if (grid[nextX][nextY] >= grid[x][y]) {
                dfs(grid, visited, nextX, nextY);
            }
        }
    }

    public List<List<Integer>> pacificAtlantic(int[][] heights) {

        n = heights.length;
        m = heights[0].length;

        // 记录能够到达太平洋的节点
        visited1 = new boolean[n][m];

        // 记录能够到达大西洋的节点
        visited2 = new boolean[n][m];

        // =========================
        // Pacific
        // 上边界 + 左边界
        // =========================

        // 左边界
        for (int i = 0; i < n; i++) {
            dfs(heights, visited1, i, 0);
        }

        // 上边界
        for (int j = 0; j < m; j++) {
            dfs(heights, visited1, 0, j);
        }

        // =========================
        // Atlantic
        // 下边界 + 右边界
        // =========================

        // 右边界
        for (int i = 0; i < n; i++) {
            dfs(heights, visited2, i, m - 1);
        }

        // 下边界
        for (int j = 0; j < m; j++) {
            dfs(heights, visited2, n - 1, j);
        }

        // =========================
        // 同时能够到达两片海洋
        // =========================

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {

                if (visited1[i][j] &&
                    visited2[i][j]) {

                    List<Integer> list = new ArrayList<>();

                    list.add(i);
                    list.add(j);

                    result.add(list);
                }
            }
        }

        return result;
    }
}
```

标准输入输出：

```java
import java.util.*;

public class Main {

    static int N, M;
    static int[][] grid;
    static boolean[][] visited1;
    static boolean[][] visited2;

    static int[][] dir = {
        {0, 1},
        {0, -1},
        {1, 0},
        {-1, 0}
    };

    public static void dfs(boolean[][] visited, int x, int y) {
        visited[x][y] = true;

        for (int i = 0; i < 4; i++) {
            int nextX = x + dir[i][0];
            int nextY = y + dir[i][1];

            if (nextX < 0 || nextX >= N ||
                nextY < 0 || nextY >= M ||
                visited[nextX][nextY]) {
                continue;
            }

            // 反向搜索：只能从低处走向高处或等高处
            if (grid[nextX][nextY] >= grid[x][y]) {
                dfs(visited, nextX, nextY);
            }
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        N = sc.nextInt();
        M = sc.nextInt();

        grid = new int[N][M];
        visited1 = new boolean[N][M];
        visited2 = new boolean[N][M];

        for (int i = 0; i < N; i++) {
            for (int j = 0; j < M; j++) {
                grid[i][j] = sc.nextInt();
            }
        }

        // 第一组边界：上边界 + 左边界
        for (int j = 0; j < M; j++) {
            dfs(visited1, 0, j);
        }

        for (int i = 0; i < N; i++) {
            dfs(visited1, i, 0);
        }

        // 第二组边界：下边界 + 右边界
        for (int j = 0; j < M; j++) {
            dfs(visited2, N - 1, j);
        }

        for (int i = 0; i < N; i++) {
            dfs(visited2, i, M - 1);
        }

        // 同时能到达两组边界的点
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < M; j++) {
                if (visited1[i][j] && visited2[i][j]) {
                    System.out.println(i + " " + j);
                }
            }
        }

        sc.close();
    }
}
```

# 827.最大人工岛

## 题目描述

给你一个大小为 `n x n` 二进制矩阵 `grid` 。**最多** 只能将一格 `0` 变成 `1` 。

返回执行此操作后，`grid` 中最大的岛屿面积是多少？

**岛屿** 由一组上、下、左、右四个方向相连的 `1` 形成。

 

**示例 1:**

```
输入: grid = [[1, 0], [0, 1]]
输出: 3
解释: 将一格0变成1，最终连通两个小岛得到面积为 3 的岛屿。
```

**示例 2:**

```
输入: grid = [[1, 1], [1, 0]]
输出: 4
解释: 将一格0变成1，岛屿的面积扩大为 4。
```

**示例 3:**

```
输入: grid = [[1, 1], [1, 1]]
输出: 4
解释: 没有0可以让我们变成1，面积依然为 4。
```

 

**提示：**

- `n == grid.length`
- `n == grid[i].length`
- `1 <= n <= 500`
- `grid[i][j]` 为 `0` 或 `1`

## 代码

```java
class Solution {

    // 地图行数和列数
    int N;
    int M;

    // 四个方向：下、上、右、左
    int[][] dir = {
        {1, 0},
        {-1, 0},
        {0, 1},
        {0, -1}
    };

    // 访问标记数组
    boolean[][] visited;

    public int dfs(int[][] grid, int x, int y, int mark) {

        // 当前岛屿面积
        int result = 1;

        // 给当前陆地打上岛屿编号
        grid[x][y] = mark;

        // 标记已访问
        visited[x][y] = true;

        // 向四个方向扩散
        for (int i = 0; i < 4; i++) {

            int nextX = x + dir[i][0];
            int nextY = y + dir[i][1];

            // 越界
            // 已访问
            // 海洋
            // 直接跳过
            if (nextX < 0 || nextY < 0 ||
                nextX >= N || nextY >= M ||
                visited[nextX][nextY] ||
                grid[nextX][nextY] == 0) {
                continue;
            }

            // 累加相邻陆地面积
            result += dfs(grid, nextX, nextY, mark);
        }

        return result;
    }

    public int largestIsland(int[][] grid) {

        N = grid.length;
        M = grid[0].length;

        // 岛屿编号从2开始
        // 因为0表示海洋，1表示未编号陆地
        int mark = 2;

        // key: 岛屿编号
        // value: 岛屿面积
        Map<Integer, Integer> islandMap = new HashMap<>();

        visited = new boolean[N][M];

        // =========================
        // 第一步：
        // 给每个岛屿编号
        // 并统计岛屿面积
        // =========================
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < M; j++) {

                if (grid[i][j] == 1 &&
                    !visited[i][j]) {

                    int area = dfs(grid, i, j, mark);

                    islandMap.put(mark, area);

                    mark++;
                }
            }
        }

        // =========================
        // 特殊情况：
        // 整张地图全是陆地
        // =========================

        int result = islandMap.getOrDefault(2, 0);

        // =========================
        // 第二步：
        // 尝试把每个海洋变成陆地
        // =========================
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < M; j++) {

                if (grid[i][j] == 0) {

                    // 当前海洋变成陆地
                    int area = 1;

                    // 防止同一个岛屿重复统计
                    Set<Integer> set = new HashSet<>();

                    // 枚举四个方向相邻岛屿
                    for (int k = 0; k < 4; k++) {

                        int nearX = i + dir[k][0];
                        int nearY = j + dir[k][1];

                        if (nearX < 0 || nearY < 0 ||
                            nearX >= N || nearY >= M) {
                            continue;
                        }

                        // 找到相邻岛屿
                        if (grid[nearX][nearY] > 1 &&
                            !set.contains(grid[nearX][nearY])) {

                            // 累加该岛屿面积
                            area += islandMap.get(grid[nearX][nearY]);

                            // 记录已经统计过
                            set.add(grid[nearX][nearY]);
                        }
                    }

                    // 更新最大岛屿面积
                    result = Math.max(result, area);
                }
            }
        }

        return result;
    }
}
```

# 463.岛屿的周长

## 题目描述

给定一个 `row x col` 的二维网格地图 `grid` ，其中：`grid[i][j] = 1` 表示陆地， `grid[i][j] = 0` 表示水域。

网格中的格子 **水平和垂直** 方向相连（对角线方向不相连）。整个网格被水完全包围，但其中恰好有一个岛屿（或者说，一个或多个表示陆地的格子相连组成的岛屿）。

岛屿中没有“湖”（“湖” 指水域在岛屿内部且不和岛屿周围的水相连）。格子是边长为 1 的正方形。网格为长方形，且宽度和高度均不超过 100 。计算这个岛屿的周长。

 

**示例 1：**

![img](./LeetCode--代码随想录(图论).assets/island.png)

```
输入：grid = [[0,1,0,0],[1,1,1,0],[0,1,0,0],[1,1,0,0]]
输出：16
解释：它的周长是上面图片中的 16 个黄色的边
```

**示例 2：**

```
输入：grid = [[1]]
输出：4
```

**示例 3：**

```
输入：grid = [[1,0]]
输出：4
```

 

**提示：**

- `row == grid.length`
- `col == grid[i].length`
- `1 <= row, col <= 100`
- `grid[i][j]` 为 `0` 或 `1`

## 代码

深度优先搜索：

```java
class Solution {

    // 四个方向：下、上、右、左
    int[][] dir = {
        {1, 0},
        {-1, 0},
        {0, 1},
        {0, -1}
    };

    // 访问标记数组
    boolean[][] visited;

    // 地图行数和列数
    int N;
    int M;

    public int dfs(int[][] grid, int x, int y) {

        // 当前连通块贡献的周长
        int result = 0;

        // 枚举当前陆地的四条边
        for (int i = 0; i < 4; i++) {

            int nextX = x + dir[i][0];
            int nextY = y + dir[i][1];

            // =========================
            // 情况1：越界
            // 当前边暴露在海洋中
            // 周长+1
            // =========================
            if (nextX < 0) result++;
            if (nextX >= N) result++;
            if (nextY < 0) result++;
            if (nextY >= M) result++;

            // =========================
            // 情况2：相邻格子是海洋
            // 当前边暴露在海洋中
            // 周长+1
            // =========================
            if (nextX >= 0 && nextX < N &&
                nextY >= 0 && nextY < M &&
                grid[nextX][nextY] == 0) {

                result++;
            }

            // =========================
            // 情况3：相邻格子是陆地
            // 且尚未访问
            // 继续DFS
            // =========================
            if (nextX >= 0 && nextX < N &&
                nextY >= 0 && nextY < M &&
                grid[nextX][nextY] == 1 &&
                !visited[nextX][nextY]) {

                visited[nextX][nextY] = true;

                result += dfs(grid, nextX, nextY);
            }
        }

        return result;
    }

    public int islandPerimeter(int[][] grid) {

        N = grid.length;
        M = grid[0].length;

        visited = new boolean[N][M];

        // 找到岛屿起点
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < M; j++) {

                if (grid[i][j] == 1) {

                    // 标记起点已访问
                    visited[i][j] = true;

                    // DFS统计整座岛屿周长
                    return dfs(grid, i, j);
                }
            }
        }

        return 0;
    }
}
```

# 110.字符串迁移

## 题目描述

字典 strList 中从字符串 beginStr 和 endStr 的转换序列是一个按下述规格形成的序列： 



1. 序列中第一个字符串是 beginStr。
2. 序列中最后一个字符串是 endStr。 
3. 每次转换只能改变一个字符。 
4. 转换过程中的中间字符串必须是字典 strList 中的字符串，且strList里的每个字符串只用使用一次。 



给你两个字符串 beginStr 和 endStr 和一个字典 strList，找到从 beginStr 到 endStr 的最短转换序列中的字符串数目。如果不存在这样的转换序列，返回 0。

输入描述

第一行包含一个整数 N，表示字典 strList 中的字符串数量。 第二行包含两个字符串，用空格隔开，分别代表 beginStr 和 endStr。 后续 N 行，每行一个字符串，代表 strList 中的字符串。

输出描述

输出一个整数，代表从 beginStr 转换到 endStr 需要的最短转换序列中的字符串数量。如果不存在这样的转换序列，则输出 0。

输入示例

```
6
abc def
efc
dbc
ebc
dec
dfc
yhn
```

输出示例

```
4
```

提示信息

从 startStr 到 endStr，在 strList 中最短的路径为 abc -> dbc -> dec -> def，所以输出结果为 4。

数据范围：

2 <= N <= 500

## 代码

```java
import java.util.*;

public class Main {

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        // 字典中的单词数量
        int n = sc.nextInt();

        // 起始单词
        String beginStr = sc.next();

        // 目标单词
        String endStr = sc.next();

        sc.nextLine();

        // 存储所有合法单词
        Set<String> set = new HashSet<>();

        for (int i = 0; i < n; i++) {
            set.add(sc.nextLine());
        }

        // 将起点和终点加入字典
        set.add(beginStr);
        set.add(endStr);

        // =========================
        // BFS最短路
        // key : 单词
        // value : 从beginStr到该单词的步数
        // =========================
        Map<String, Integer> map = new HashMap<>();

        // 起点记为第1步
        map.put(beginStr, 1);

        // BFS队列
        Queue<String> que = new ArrayDeque<>();

        // 起点入队
        que.add(beginStr);

        // 标记是否找到答案
        boolean flag = true;

        while (!que.isEmpty()) {

            // 当前单词
            String str = que.poll();

            // 到达终点
            if (endStr.equals(str)) {

                flag = false;

                System.out.println(map.get(str));

                break;
            }

            // =========================
            // 枚举所有可能的下一状态
            // =========================

            // 遍历单词的每一个字符位置
            for (int i = 0; i < str.length(); i++) {

                // 每次重新复制一份字符数组
                char[] newChs = str.toCharArray();

                // 当前位置尝试替换成26个小写字母
                for (int j = 0; j < 26; j++) {

                    newChs[i] = (char) ('a' + j);

                    String newStr = new String(newChs);

                    // 新单词合法
                    // 并且之前没有访问过
                    if (set.contains(newStr)
                        && !map.containsKey(newStr)) {

                        // 步数+1
                        map.put(newStr, map.get(str) + 1);

                        // 加入队列继续搜索
                        que.add(newStr);
                    }
                }
            }
        }

        // 无法到达终点
        if (flag) {
            System.out.println(0);
        }

        sc.close();
    }
}
```

# 105.有向图的完全联通

## 题目描述

给定一个有向图，包含 N 个节点，节点编号分别为 1，2，...，N。现从 1 号节点开始，如果可以从 1 号节点的边可以到达任何节点，则输出 1，否则输出 -1。

输入描述

第一行包含两个正整数，表示节点数量 N 和边的数量 K。 后续 K 行，每行两个正整数 s 和 t，表示从 s 节点有一条边单向连接到 t 节点。

输出描述

如果可以从 1 号节点的边可以到达任何节点，则输出 1，否则输出 -1。

输入示例

```
4 4
1 2
2 1
1 3
2 4
```

输出示例

```
1
```

提示信息

![img](./LeetCode--代码随想录(图论).assets/20240415192546_54466.png)



从 1 号节点可以到达任意节点，输出 1。



**数据范围：**

1 <= N <= 100；
1 <= K <= 2000。

## 代码

```java
import java.util.*;

public class Main {

    // 记录节点是否已经访问过
    public static boolean[] visited;

    // 邻接表
    // table[i] 存储节点i能够到达的所有节点
    public static List<Integer>[] table;

    // 记录从节点1出发能够访问到的节点数量
    public static int result = 0;

    public static void dfs(int cur) {

        // 当前节点加入连通块
        result++;

        // 标记当前节点已访问
        visited[cur] = true;

        // 遍历当前节点的所有邻接节点
        for (int next : table[cur]) {

            // 对于有向图实际上不需要parent判断
            // visited已经能够避免重复访问

            if (!visited[next]) {
                dfs(next);
            }
        }
    }

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        // 节点数量
        int N = sc.nextInt();

        // 边数量
        int K = sc.nextInt();

        visited = new boolean[N + 1];

        // 创建邻接表
        table = new ArrayList[N + 1];

        for (int i = 0; i <= N; i++) {
            table[i] = new ArrayList<>();
        }

        // 读入有向边
        for (int i = 0; i < K; i++) {

            int start = sc.nextInt();
            int end = sc.nextInt();

            // start -> end
            table[start].add(end);
        }

        // 从节点1开始DFS
        dfs(1);

        // 如果访问到所有节点
        // 说明从1出发能够到达整个图
        if (result == N) {
            System.out.println(1);
        } else {
            System.out.println(-1);
        }

        sc.close();
    }
}
```

# 1971.寻找图中是否存在路径

## 题目描述

有一个具有 `n` 个顶点的 **双向** 图，其中每个顶点标记从 `0` 到 `n - 1`（包含 `0` 和 `n - 1`）。图中的边用一个二维整数数组 `edges` 表示，其中 `edges[i] = [ui, vi]` 表示顶点 `ui` 和顶点 `vi` 之间的双向边。 每个顶点对由 **最多一条** 边连接，并且没有顶点存在与自身相连的边。

请你确定是否存在从顶点 `source` 开始，到顶点 `destination` 结束的 **有效路径** 。

给你数组 `edges` 和整数 `n`、`source` 和 `destination`，如果从 `source` 到 `destination` 存在 **有效路径** ，则返回 `true`，否则返回 `false` 。

 

**示例 1：**

![img](./LeetCode--代码随想录(图论).assets/validpath-ex1.png)

```
输入：n = 3, edges = [[0,1],[1,2],[2,0]], source = 0, destination = 2
输出：true
解释：存在由顶点 0 到顶点 2 的路径:
- 0 → 1 → 2 
- 0 → 2
```

**示例 2：**

![img](./LeetCode--代码随想录(图论).assets/validpath-ex2.png)

```
输入：n = 6, edges = [[0,1],[0,2],[3,5],[5,4],[4,3]], source = 0, destination = 5
输出：false
解释：不存在由顶点 0 到顶点 5 的路径.
```

 

**提示：**

- `1 <= n <= 2 * 105`
- `0 <= edges.length <= 2 * 105`
- `edges[i].length == 2`
- `0 <= ui, vi <= n - 1`
- `ui != vi`
- `0 <= source, destination <= n - 1`
- 不存在重复边
- 不存在指向顶点自身的边

## 代码

力扣：

```java
class Solution {

    // father[i] 表示节点i的父节点
    int[] father;

    // 初始化并查集
    // 每个节点的父节点都是自己
    void init(int n) {

        father = new int[n];

        for (int i = 0; i < n; i++) {
            father[i] = i;
        }
    }

    // 合并两个集合
    void join(int u, int v) {

        // 找到两个节点所在集合的根节点
        u = find(u);
        v = find(v);

        // 不属于同一个集合时进行合并
        if (u != v) {
            father[u] = v;
        }
    }

    // 查找节点所在集合的根节点
    // 使用路径压缩优化
    int find(int u) {

        // 找到根节点
        if (u == father[u]) {
            return u;
        }

        // 路径压缩
        father[u] = find(father[u]);

        return father[u];
    }

    public boolean validPath(int n,
                             int[][] edges,
                             int source,
                             int destination) {

        // 初始化并查集
        init(n);

        // 遍历所有边
        for (int i = 0; i < edges.length; i++) {

            int u = edges[i][0];
            int v = edges[i][1];

            // 将两个节点所在集合合并
            join(u, v);
        }

        // 判断起点和终点是否属于同一个集合
        return find(source) == find(destination);
    }
}
```

并查集实现（ACM模式）：

```java
import java.util.*;

public class Main {

    // 节点数量
    public static int N;

    // father[i] 表示节点i的父节点
    public static int[] father;

    // 初始化并查集
    // 每个节点的父节点都是自己
    public static void init() {

        for (int i = 0; i <= N; i++) {
            father[i] = i;
        }
    }

    // 查找节点所在集合的根节点
    // 路径压缩优化
    public static int find(int u) {

        // 找到根节点
        if (u == father[u]) {
            return u;
        }

        // 路径压缩
        father[u] = find(father[u]);

        return father[u];
    }

    // 合并两个集合
    public static void join(int u, int t) {

        // 找到两个节点的根节点
        u = find(u);
        t = find(t);

        // 已经属于同一个集合
        if (u == t) {
            return;
        }

        // 将u所在集合挂到t所在集合下面
        father[u] = t;
    }

    // 判断两个节点是否属于同一个集合
    public static boolean isSame(int u, int t) {

        u = find(u);
        t = find(t);

        return u == t;
    }

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        // 节点数量
        N = sc.nextInt();

        // 并查集数组
        father = new int[N + 1];

        // 初始化并查集
        init();

        // 边数量
        int M = sc.nextInt();

        // 读入无向图的所有边
        for (int i = 0; i < M; i++) {

            int s = sc.nextInt();
            int t = sc.nextInt();

            // 将两个节点所在集合合并
            join(s, t);
        }

        // 起点
        int source = sc.nextInt();

        // 终点
        int destination = sc.nextInt();

        // 判断两个节点是否连通
        boolean result = isSame(source, destination);

        // 输出结果
        if (result) {
            System.out.println(1);
        } else {
            System.out.println(0);
        }

        sc.close();
    }
}
```

DFS实现（ACM模式）：

```java
import java.util.*;
 
public class Main {
 
    // 访问标记数组
    public static boolean[] visited;
 
    // 是否找到目标节点
    public static boolean isFound = false;
 
    // 节点数量
    public static int N;
 
    // 邻接矩阵
    // graph[i][j] = 1 表示存在边 i -> j
    public static int[][] graph;
 
    public static void dfs(int source, int destination) {
 
        if(isFound) return;
 
        if(source == destination){
            isFound = true;
            return;
        }
 
        // 标记当前节点已访问
        visited[source] = true;
 
        // 枚举当前节点能够到达的所有节点
        for(int i = 1; i <= N; i++){
            if(graph[source][i] == 1 && !visited[i]){
                dfs(i, destination);
            }
        }
    }
 
    public static void main(String[] args) {
 
        Scanner sc = new Scanner(System.in);
 
        // 节点数量
        N = sc.nextInt();
 
        // 边数量
        int M = sc.nextInt();
 
        graph = new int[N + 1][N + 1];
 
        visited = new boolean[N + 1];
 
        // 构建无向图
        for (int i = 0; i < M; i++) {
 
            int s = sc.nextInt();
            int t = sc.nextInt();
 
            graph[s][t] = 1;
            graph[t][s] = 1;
        }
 
        // 起点
        int source = sc.nextInt();
 
        // 终点
        int destination = sc.nextInt();
 
        // DFS判断连通性
        dfs(source, destination);
 
        // 输出结果
        if (isFound) {
            System.out.println(1);
        } else {
            System.out.println(0);
        }
 
        sc.close();
    }
}
```

# 684. 冗余连接

## 题目描述

树可以看成是一个连通且 **无环** 的 **无向** 图。

给定一个图，该图从一棵 `n` 个节点 (节点值 `1～n`) 的树中添加一条边后获得。添加的边的两个不同顶点编号在 `1` 到 `n` 中间，且这条附加的边不属于树中已存在的边。图的信息记录于长度为 `n` 的二维数组 `edges` ，`edges[i] = [ai, bi]` 表示图中在 `ai` 和 `bi` 之间存在一条边。

请找出一条可以删去的边，删除后可使得剩余部分是一个有着 `n` 个节点的树。如果有多个答案，则返回数组 `edges` 中最后出现的那个。

 

**示例 1：**

![img](./LeetCode--代码随想录(图论).assets/1626676174-hOEVUL-image.png)

```
输入: edges = [[1,2], [1,3], [2,3]]
输出: [2,3]
```

**示例 2：**

![img](./LeetCode--代码随想录(图论).assets/1626676179-kGxcmu-image.png)

```
输入: edges = [[1,2], [2,3], [3,4], [1,4], [1,5]]
输出: [1,4]
```

 

**提示:**

- `n == edges.length`
- `3 <= n <= 1000`
- `edges[i].length == 2`
- `1 <= ai < bi <= edges.length`
- `ai != bi`
- `edges` 中无重复元素
- 给定的图是连通的 

## 代码

```java
class Solution {

    // 节点数量
    int N;

    // 并查集父节点数组
    int[] father;

    // 初始化并查集
    void init() {

        father = new int[N + 1];

        for (int i = 0; i <= N; i++) {
            father[i] = i;
        }
    }

    // 查找根节点
    // 路径压缩
    int find(int u) {

        if (u == father[u]) {
            return u;
        }

        father[u] = find(father[u]);

        return father[u];
    }

    // 合并两个集合
    void join(int u, int v) {

        u = find(u);
        v = find(v);

        if (u != v) {
            father[u] = father[v];
        }
    }

    public int[] findRedundantConnection(int[][] edges) {

        // LeetCode保证：
        // 节点数 = 边数
        N = edges.length;

        // 保存最终答案
        int[] result = new int[2];

        init();

        // 依次加入每条边
        for (int i = 0; i < N; i++) {

            int s = edges[i][0];
            int t = edges[i][1];

            // 如果两个节点已经连通
            // 当前边会形成环
            if (find(s) == find(t)) {

                result = edges[i];
            }

            // 合并两个连通块
            join(s, t);
        }

        return result;
    }
}
```

ACM模式：

```java
import java.util.*;

public class Main {

    // 节点数量
    public static int N;

    // 并查集父节点数组
    public static int[] father;

    // 初始化并查集
    // 每个节点的父节点都是自己
    public static void init() {

        father = new int[N + 1];

        for (int i = 0; i <= N; i++) {
            father[i] = i;
        }
    }

    // 查找节点所在集合的根节点
    // 路径压缩优化
    public static int find(int u) {

        if (u == father[u]) {
            return u;
        }

        father[u] = find(father[u]);

        return father[u];
    }

    // 合并两个集合
    public static void join(int u, int v) {

        u = find(u);
        v = find(v);

        if (u != v) {
            father[u] = father[v];
        }
    }

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        // 节点数量
        N = sc.nextInt();

        // 记录最终需要删除的边
        int deleteSource = 0;
        int deleteDestination = 0;

        init();

        // 题目共有N条边
        // 原本树有N-1条边
        // 多出来的一条边会形成环
        for (int i = 0; i < N; i++) {

            int s = sc.nextInt();
            int t = sc.nextInt();

            // 如果两个节点已经连通
            // 再连接就会形成环
            if (find(s) == find(t)) {

                deleteSource = s;
                deleteDestination = t;
            }

            // 合并两个节点所在集合
            join(s, t);
        }

        // 输出冗余边
        System.out.println(
            deleteSource + " " + deleteDestination
        );

        sc.close();
    }
}
```

# 685. 冗余连接 II

## 题目描述

在本问题中，有根树指满足以下条件的 **有向** 图。该树只有一个根节点，所有其他节点都是该根节点的后继。该树除了根节点之外的每一个节点都有且只有一个父节点，而根节点没有父节点。

输入一个有向图，该图由一个有着 `n` 个节点（节点值不重复，从 `1` 到 `n`）的树及一条附加的有向边构成。附加的边包含在 `1` 到 `n` 中的两个不同顶点间，这条附加的边不属于树中已存在的边。

结果图是一个以边组成的二维数组 `edges` 。 每个元素是一对 `[ui, vi]`，用以表示 **有向** 图中连接顶点 `ui` 和顶点 `vi` 的边，其中 `ui` 是 `vi` 的一个父节点。

返回一条能删除的边，使得剩下的图是有 `n` 个节点的有根树。若有多个答案，返回最后出现在给定二维数组的答案。

 

**示例 1：**

![img](./LeetCode--代码随想录(图论).assets/graph1.jpg)

```
输入：edges = [[1,2],[1,3],[2,3]]
输出：[2,3]
```

**示例 2：**

![img](./LeetCode--代码随想录(图论).assets/graph2.jpg)

```
输入：edges = [[1,2],[2,3],[3,4],[4,1],[1,5]]
输出：[4,1]
```

 

**提示：**

- `n == edges.length`
- `3 <= n <= 1000`
- `edges[i].length == 2`
- `1 <= ui, vi <= n`

## 图解思路

![image-20260613214030230](./LeetCode--代码随想录(图论).assets/image-20260613214030230.png)

## 代码

```java
class Solution {

    // 节点数量
    int N;

    // 入度统计数组
    int[] indegree;

    // 保存所有边
    Pair[] pairs;

    // 并查集父节点数组
    int[] father;

    // 初始化并查集
    void init() {
        for (int i = 0; i <= N; i++) {
            father[i] = i;
        }
    }

    // 合并两个集合
    void join(int u, int v) {

        u = find(u);
        v = find(v);

        if (u != v) {
            father[u] = v;
        }
    }

    // 查找根节点
    // 路径压缩
    int find(int u) {

        if (u == father[u]) {
            return u;
        }

        father[u] = find(father[u]);

        return father[u];
    }

    // 判断两个节点是否已经连通
    boolean isSame(int u, int v) {
        return find(u) == find(v);
    }

    // 保存一条边
    class Pair {

        int first;
        int second;

        Pair(int first, int second) {
            this.first = first;
            this.second = second;
        }
    }

    /**
     * 删除第cur条边后
     * 判断剩余图是否构成合法树
     */
    boolean isValid(int cur, int[][] edges) {

        init();

        for (int i = 0; i < N; i++) {

            // 跳过待删除边
            if (i == cur) {
                continue;
            }

            // 出现环
            if (isSame(edges[i][0], edges[i][1])) {
                return false;
            }

            join(edges[i][0], edges[i][1]);
        }

        return true;
    }

    public int[] findRedundantDirectedConnection(int[][] edges) {

        N = edges.length;

        indegree = new int[N + 1];

        pairs = new Pair[N + 1];

        father = new int[N + 1];

        // 是否存在入度为2的节点
        boolean isTwoIn = false;

        // 入度为2的节点编号
        int twoNum = 0;

        int[] result = new int[2];

        // =========================
        // 统计每个节点入度
        // =========================
        for (int i = 0; i < N; i++) {

            int first = edges[i][0];
            int second = edges[i][1];

            pairs[i + 1] = new Pair(first, second);

            indegree[second]++;

            // 找到入度为2的节点
            if (indegree[second] == 2) {

                isTwoIn = true;
                twoNum = second;
            }
        }

        // =========================
        // 情况1：
        // 存在入度为2的节点
        // =========================
        if (isTwoIn) {

            // 从后向前找
            // 符合题目要求：
            // 返回最后出现的答案
            for (int i = N; i > 0; i--) {

                if (pairs[i].second == twoNum) {

                    // 删除该边后成为合法树
                    if (isValid(i - 1, edges)) {

                        result[0] = pairs[i].first;
                        result[1] = pairs[i].second;

                        break;
                    }
                }
            }

        } else {

            // =========================
            // 情况2：
            // 不存在入度为2
            // 只可能是环
            // =========================
            for (int i = N; i > 0; i--) {

                if (isValid(i - 1, edges)) {

                    result[0] = pairs[i].first;
                    result[1] = pairs[i].second;

                    break;
                }
            }
        }

        return result;
    }
}
```

# 53.寻宝

## 题目描述

在世界的某个区域，有一些分散的神秘岛屿，每个岛屿上都有一种珍稀的资源或者宝藏。国王打算在这些岛屿上建公路，方便运输。

不同岛屿之间，路途距离不同，国王希望你可以规划建公路的方案，如何可以以最短的总公路距离将 所有岛屿联通起来（注意：这是一个无向图）。 

给定一张地图，其中包括了所有的岛屿，以及它们之间的距离。以最小化公路建设长度，确保可以链接到所有岛屿。

输入描述

第一行包含两个整数V 和 E，V代表顶点数，E代表边数 。顶点编号是从1到V。例如：V=2，一个有两个顶点，分别是1和2。

接下来共有 E 行，每行三个整数 v1，v2 和 val，v1 和 v2 为边的起点和终点，val代表边的权值。

输出描述

输出联通所有岛屿的最小路径总距离

输入示例

```
7 11
1 2 1
1 3 1
1 5 2
2 6 1
2 4 2
2 3 2
3 4 1
4 5 1
5 6 2
5 7 1
6 7 1
```

输出示例

```
6
```

提示信息

数据范围：

2 <= V <= 10000;
1 <= E <= 100000;
0 <= val <= 10000;

如下图，可见将所有的顶点都访问一遍，总距离最低是6.

 ![img](./LeetCode--代码随想录(图论).assets/20230919201506_90440.png)

## 代码

```java
import java.util.*;

public class Main{

    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);

        // 将所有输入读进邻接矩阵
        int V = sc.nextInt();
        int E = sc.nextInt();

        int[][] graph = new int[V+1][V+1];
        int max = Integer.MAX_VALUE;

        for(int i=0; i<E; i++){
            int s = sc.nextInt();
            int t = sc.nextInt();
            int weight = sc.nextInt();
            
            graph[s][t] = weight;
            graph[t][s] = weight;
            
        }
        
        // minDist 数组用于存放非树节点的最小距离
        // isInTree 数组用于存放树节点
        int[] minDist = new int[V+1];
        boolean[] isInTree = new boolean[V+1];

        // 最小距离全部初始化为 max
        Arrays.fill(minDist, max);

        int cur = 1;
        isInTree[cur] = true;

        // 最小生成树的边个数为顶点数-1
        for(int i=1; i<V; i++){
            
            // 在非树节点中记录当前距离树节点最近的节点距离
            for(int j=1; j<=V; j++){
                // 如果当前距离小于minDist中记录的最小距离，更新minDist
                if(graph[cur][j] > 0 && !isInTree[j] && minDist[j] > graph[cur][j]){
                    minDist[j] = graph[cur][j];
                }
            }

            int min = max;
            int index = 0;
            // 找到非树节点中最近的加入树节点
            for(int j=1; j<=V; j++){
                if(min > minDist[j] && !isInTree[j]){
                    min = minDist[j];
                    index = j;
                }
            }
            isInTree[index] = true;
            
            // 从新加入的树节点重新统计
            cur = index;
        } 

        int result = 0;

        for(int i=2; i<=V; i++){
            result += minDist[i];
        }

        System.out.println(result);
        
        sc.close();
    }

}
```

# 1584. 连接所有点的最小费用

## 题目描述

给你一个`points` 数组，表示 2D 平面上的一些点，其中 `points[i] = [xi, yi]` 。

连接点 `[xi, yi]` 和点 `[xj, yj]` 的费用为它们之间的 **曼哈顿距离** ：`|xi - xj| + |yi - yj|` ，其中 `|val|` 表示 `val` 的绝对值。

请你返回将所有点连接的最小总费用。只有任意两点之间 **有且仅有** 一条简单路径时，才认为所有点都已连接。

 

**示例 1：**

![img](./LeetCode--代码随想录(图论).assets/d.png)

```
输入：points = [[0,0],[2,2],[3,10],[5,2],[7,0]]
输出：20
解释：

我们可以按照上图所示连接所有点得到最小总费用，总费用为 20 。
注意到任意两个点之间只有唯一一条路径互相到达。
```

**示例 2：**

```
输入：points = [[3,12],[-2,5],[-4,1]]
输出：18
```

**示例 3：**

```
输入：points = [[0,0],[1,1],[1,0],[-1,1]]
输出：4
```

**示例 4：**

```
输入：points = [[-1000000,-1000000],[1000000,1000000]]
输出：4000000
```

**示例 5：**

```
输入：points = [[0,0]]
输出：0
```

 

**提示：**

- `1 <= points.length <= 1000`
- `-106 <= xi, yi <= 106`
- 所有点 `(xi, yi)` 两两不同。

## 图解思路

最小生成树，可以用Kruskal或者Prim解决。

![image-20260614201447979](./LeetCode--代码随想录(图论).assets/image-20260614201447979.png)

## 代码

Kruskal算法：

```java
class Solution {
    /**
    * LeetCode 1584 连接所有点的最小费用
    * Kruskal最小生成树算法实现
    * 本题是完全稠密图，Kruskal需要生成n*(n-1)/2条边，数据量大时效率不如堆优化Prim，但逻辑直观好写
    */
    
    // 边结构体：存储一条边的两个端点、边权（曼哈顿距离）
    class Edge{
        int startPoint; // 起点下标
        int endPoint;   // 终点下标
        int val;        // 两点之间曼哈顿距离权重
    }

    int[] father; // 并查集父节点数组

    /**
    * 初始化并查集
    * 每个点初始父节点是自己，各自独立成一个连通块
    * @param n 总点数
    */
    void init(int n){
        father = new int[n];
        for(int i = 0; i < n; i++){
            father[i] = i;
        }
    }

    /**
    * 合并两个连通块
    * 先找两个点的根节点，根不同则把其中一个根挂到另一个根下面
    * @param u 点u
    * @param v 点v
    */
    void join(int u, int v){
        u = find(u);
        v = find(v);

        if(u != v){
            father[v] = u;
        }
    }

    /**
    * 查找根节点 + 路径压缩优化
    * @param u 当前查询点
    * @return u所在集合的根节点
    */
    int find(int u){
        // 自身等于父节点说明是根
        if(u == father[u]) return u;
        // 递归找根，顺带把路径上所有节点直接指向根（路径压缩）
        father[u] = find(father[u]);
        return father[u];
    }

    /**
    * 判断两点是否属于同一个连通块
    * @param u 点u
    * @param v 点v
    * @return 同集合返回true，不同false
    */
    boolean isSame(int u, int v){
        return find(u) == find(v);
    }

    public int minCostConnectPoints(int[][] points) {
        int result = 0; // 累加最小生成树总权值

        int n = points.length; // 一共有n个坐标点
        init(n); // 初始化并查集
        // 完全无向图总边数公式：n*(n-1)/2
        int edgeNum = (n)*(n-1)/2;
        List<Edge> edges = new ArrayList<>(); // 存放所有两点间的边

        // 双重循环枚举每一对点，生成所有无向边（i<j避免重复建边）
        for(int i = 0; i < n - 1; i++){
            for(int j = i + 1; j < n; j++){
                Edge edge = new Edge();
                edge.startPoint = i;
                edge.endPoint = j;
                // 计算曼哈顿距离作为边权重
                int dx = Math.abs(points[i][0] - points[j][0]);
                int dy = Math.abs(points[i][1] - points[j][1]);
                edge.val = dx + dy;
                edges.add(edge);
            }
        }

        // Kruskal核心：把所有边按权重从小到大升序排序（贪心选最小边）
        Collections.sort(edges, (a, b) -> a.val - b.val);

        // 依次遍历每条边，能合并不同连通块就加入MST
        for(int i = 0; i < edgeNum; i++){
            Edge curEdge = edges.get(i);
            // 两个端点不在同一集合，连接不会成环，可以收录这条边
            if(!isSame(curEdge.startPoint, curEdge.endPoint)){
                join(curEdge.startPoint, curEdge.endPoint);
                result += curEdge.val;
            }
        }

        return result;
    }
}
```

Prim算法：

```java
class Solution {
    /**
     *  Prim算法实现
     */

    public int minCostConnectPoints(int[][] points) {
        // 累加最小生成树的总花费
        int result = 0;
        // 点的总数量
        int n = points.length;
        // isInTree[i]：标记点i是否已经被纳入最小生成树集合
        boolean[] isInTree = new boolean[n];
        
        // minDist[j]：点j到当前生成树集合的最短曼哈顿距离
        int[] minDist = new int[n];
        // 初始化所有距离为无穷大
        Arrays.fill(minDist, Integer.MAX_VALUE);

        // 选取0号点作为生成树初始起点，先放进树里
        isInTree[0] = true;
        // cur 记录上一轮刚刚加入树的节点
        int cur = 0;

        // 生成树总共需要n个点，已经放了0，还需要再加入n-1个点，循环n-1次
        for(int i = 1; i < n; i++){
            // 只用刚新增进树的cur点，更新所有不在树中的点到树的最短距离
            for(int j = 0; j < n; j++){
                // j还没有加入生成树
                if(!isInTree[j]){
                    // cur是树内节点，计算cur到j的距离，尝试更新j的最小距离
                    if(cur != j && isInTree[cur]){
                        int dx = Math.abs(points[cur][0] - points[j][0]);
                        int dy = Math.abs(points[cur][1] - points[j][1]);
                        int dist = dx + dy;
                        // 如果当前距离比记录的更小，就刷新minDist[j]
                        minDist[j] = Math.min(minDist[j], dist);
                    }
                }
            }

            // 遍历全部点，找出不在树中、minDist值最小的那个点
            int minVal = Integer.MAX_VALUE;
            for(int l = 0; l < n; l++){
                // 只看未入树的节点
                if(!isInTree[l] && minVal > minDist[l]){
                    minVal = minDist[l];
                    cur = l; // 把这个最近点赋值给cur，下一轮用它更新距离
                }
            }

            // 将找到的最近点正式加入生成树
            isInTree[cur] = true;
            // 把这条最小边的权值计入总费用
            result += minVal;
        }

        return result;
    }
}
```

# 210. 课程表 II

## 题目描述

现在你总共有 `numCourses` 门课需要选，记为 `0` 到 `numCourses - 1`。给你一个数组 `prerequisites` ，其中 `prerequisites[i] = [ai, bi]` ，表示在选修课程 `ai` 前 **必须** 先选修 `bi` 。

- 例如，想要学习课程 `0` ，你需要先完成课程 `1` ，我们用一个匹配来表示：`[0,1]` 。

返回你为了学完所有课程所安排的学习顺序。可能会有多个正确的顺序，你只要返回 **任意一种** 就可以了。如果不可能完成所有课程，返回 **一个空数组** 。

 

**示例 1：**

```
输入：numCourses = 2, prerequisites = [[1,0]]
输出：[0,1]
解释：总共有 2 门课程。要学习课程 1，你需要先完成课程 0。因此，正确的课程顺序为 [0,1] 。
```

**示例 2：**

```
输入：numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]
输出：[0,2,1,3]
解释：总共有 4 门课程。要学习课程 3，你应该先完成课程 1 和课程 2。并且课程 1 和课程 2 都应该排在课程 0 之后。
因此，一个正确的课程顺序是 [0,1,2,3] 。另一个正确的排序是 [0,2,1,3] 。
```

**示例 3：**

```
输入：numCourses = 1, prerequisites = []
输出：[0]
```

 

**提示：**

- `1 <= numCourses <= 2000`
- `0 <= prerequisites.length <= numCourses * (numCourses - 1)`
- `prerequisites[i].length == 2`
- `0 <= ai, bi < numCourses`
- `ai != bi`
- 所有`[ai, bi]` **互不相同**

## 图解思路

![image-20260615121955582](./LeetCode--代码随想录(图论).assets/image-20260615121955582.png)

## 代码

力扣：

```java
class Solution {

    // indegree[i]：课程 i 当前还剩多少个前置课程未完成
    int[] indegree;

    public int[] findOrder(int numCourses, int[][] prerequisites) {

        int n = prerequisites.length;

        indegree = new int[numCourses];

        // 存储最终的拓扑排序结果
        int[] result = new int[numCourses];

        // 邻接表
        // graph[u] 存储所有由 u 指向的节点
        //
        // 例如：
        // [1, 0]
        // 表示课程1依赖课程0
        // 建边：0 -> 1
        //
        // graph[0] = [1]
        List<Integer>[] graph = new ArrayList[numCourses];

        // 建图 + 统计入度
        for (int i = 0; i < n; i++) {

            // 边的终点（被依赖课程）
            int child = prerequisites[i][0];

            // 边的起点（前置课程）
            int parent = prerequisites[i][1];

            // 延迟初始化邻接表
            if (graph[parent] == null) {
                graph[parent] = new ArrayList<>();
            }

            // parent -> child
            graph[parent].add(child);

            // child 多了一个前置课程
            indegree[child]++;
        }

        Queue<Integer> que = new ArrayDeque<>();

        // 所有入度为0的节点入队
        // 表示这些课程没有前置依赖，可以直接学习
        for (int i = 0; i < numCourses; i++) {
            if (indegree[i] == 0) {
                que.add(i);
            }
        }

        int index = 0;

        // Kahn算法（BFS拓扑排序）
        while (!que.isEmpty()) {

            // 当前可学习课程
            int node = que.poll();

            // 获取当前课程的所有后继课程
            List<Integer> list = graph[node];

            if (list != null) {

                for (int next : list) {

                    // 完成当前课程后
                    // 后继课程少了一个前置依赖
                    indegree[next]--;

                    // 若入度变为0
                    // 说明其所有前置课程均已完成
                    if (indegree[next] == 0) {
                        que.add(next);
                    }
                }
            }

            // 当前课程加入拓扑序列
            result[index++] = node;
        }

        // 若拓扑序列长度等于课程总数
        // 说明图中无环，返回合法学习顺序
        //
        // 否则存在环，无法完成所有课程
        return numCourses == index ? result : new int[0];
    }
}
```

ACM输入输出模式：

```java
import java.util.*;

class Solution {

    // indegree[i]：课程 i 当前还剩多少个前置课程未完成
    int[] indegree;

    public int[] findOrder(int numCourses, int[][] prerequisites) {

        int n = prerequisites.length;

        indegree = new int[numCourses];

        // 存储最终的拓扑排序结果
        int[] result = new int[numCourses];

        // 邻接表
        // graph[u] 存储所有由 u 指向的节点
        //
        // 例如：
        // [1, 0]
        // 表示课程1依赖课程0
        // 建边：0 -> 1
        //
        // graph[0] = [1]
        List<Integer>[] graph = new ArrayList[numCourses];

        // 建图 + 统计入度
        for (int i = 0; i < n; i++) {

            // 边的终点（被依赖课程）
            int child = prerequisites[i][0];

            // 边的起点（前置课程）
            int parent = prerequisites[i][1];

            // 延迟初始化邻接表
            if (graph[parent] == null) {
                graph[parent] = new ArrayList<>();
            }

            // parent -> child
            graph[parent].add(child);

            // child 多了一个前置课程
            indegree[child]++;
        }

        Queue<Integer> que = new ArrayDeque<>();

        // 所有入度为0的节点入队
        // 表示这些课程没有前置依赖，可以直接学习
        for (int i = 0; i < numCourses; i++) {
            if (indegree[i] == 0) {
                que.add(i);
            }
        }

        int index = 0;

        // Kahn算法（BFS拓扑排序）
        while (!que.isEmpty()) {

            // 当前可学习课程
            int node = que.poll();

            // 获取当前课程的所有后继课程
            List<Integer> list = graph[node];

            if (list != null) {

                for (int next : list) {

                    // 完成当前课程后
                    // 后继课程少了一个前置依赖
                    indegree[next]--;

                    // 若入度变为0
                    // 说明其所有前置课程均已完成
                    if (indegree[next] == 0) {
                        que.add(next);
                    }
                }
            }

            // 当前课程加入拓扑序列
            result[index++] = node;
        }

        // 若拓扑序列长度等于课程总数
        // 说明图中无环，返回合法学习顺序
        //
        // 否则存在环，无法完成所有课程
        return numCourses == index ? result : new int[0];
    }
}

public class Main{
    

    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int numCourses = sc.nextInt();
        int dependencies = sc.nextInt();

        int[][] prerequisites = new int[dependencies][2];

        for(int i=0; i<dependencies; i++){
            int s = sc.nextInt();
            int t = sc.nextInt();
            
            prerequisites[i][0] = t;
            prerequisites[i][1] = s;

        }

        Solution solution = new Solution();

        int[] result = solution.findOrder(numCourses, prerequisites);
        if(result.length<2){
            System.out.println(-1);
        } else{
            for(int i=0; i<result.length-1; i++){
                System.out.print(result[i] + " ");
            }
            System.out.println(result[result.length-1]);
        }

        sc.close();

    }

}

```

# 47.参加科学大会

## 题目描述

小明是一位科学家，他需要参加一场重要的国际科学大会，以展示自己的最新研究成果。

小明的起点是第一个车站，终点是最后一个车站。然而，途中的各个车站之间的道路状况、交通拥堵程度以及可能的自然因素（如天气变化）等不同，这些因素都会影响每条路径的通行时间。

小明希望能选择一条花费时间最少的路线，以确保他能够尽快到达目的地。

输入描述

第一行包含两个正整数，第一个正整数 N 表示一共有 N 个公共汽车站，第二个正整数 M 表示有 M 条公路。 

接下来为 M 行，每行包括三个整数，S、E 和 V，代表了从 S 车站可以单向直达 E 车站，并且需要花费 V 单位的时间。

输出描述

输出一个整数，代表小明从起点到终点所花费的最小时间。

输入示例

```
7 9
1 2 1
1 3 4
2 3 2
2 4 5
3 4 2
4 5 3
2 6 4
5 7 4
6 7 9
```

输出示例

```
12
```

提示信息

**能够到达的情况：**

如下图所示，起始车站为 1 号车站，终点车站为 7 号车站，绿色路线为最短的路线，路线总长度为 12，则输出 12。



![img](./LeetCode--代码随想录(图论).assets/20240122163716_71030.png)

**
**

**不能到达的情况：**

如下图所示，当从起始车站不能到达终点车站时，则输出 -1。



![img](./LeetCode--代码随想录(图论).assets/20240125154052_26956.png)



数据范围：

1 <= N <= 500;
1 <= M <= 5000;

## 图解思路

![image-20260615162400079](./LeetCode--代码随想录(图论).assets/image-20260615162400079.png)![image-20260615162406208](./LeetCode--代码随想录(图论).assets/image-20260615162406208.png)

## 代码

```java
import java.util.*;

public class Main {

    // visited[i] = true 表示节点 i 已经加入最短路径集合
    public static boolean[] visited;

    // minDist[i] 表示从起点 1 到节点 i 的当前最短距离
    public static int[] minDist;

    // 邻接矩阵
    // graph[u][v] 表示 u -> v 的边权
    public static int[][] graph;

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        // N 个节点，M 条边
        int N = sc.nextInt();
        int M = sc.nextInt();

        graph = new int[N + 1][N + 1];
        visited = new boolean[N + 1];
        minDist = new int[N + 1];

        // 初始认为所有点不可达
        Arrays.fill(minDist, Integer.MAX_VALUE);

        // 初始化邻接矩阵
        // Integer.MAX_VALUE 表示两点之间没有边
        for (int i = 0; i <= N; i++) {
            for (int j = 0; j <= N; j++) {
                graph[i][j] = Integer.MAX_VALUE;
            }
        }

        // 读入边
        for (int i = 0; i < M; i++) {
            int S = sc.nextInt();
            int E = sc.nextInt();
            int V = sc.nextInt();

            graph[S][E] = V;
        }

        // 起点为 1
        minDist[1] = 0;

        // Dijkstra 主循环
        // 每次确定一个节点的最短路
        for (int i = 1; i <= N; i++) {

            // 当前要加入最短路径集合的节点
            int cur = 0;

            // 当前最小距离
            int minVal = Integer.MAX_VALUE;

            // 找出未访问节点中距离起点最近的节点
            for (int j = 1; j <= N; j++) {
                if (minDist[j] < minVal && !visited[j]) {
                    minVal = minDist[j];
                    cur = j;
                }
            }

            // 将该节点加入最短路径集合
            visited[cur] = true;

            // 用 cur 松弛其它节点
            for (int j = 1; j <= N; j++) {

                // 条件：
                // 1. cur -> j 有边
                // 2. 经过 cur 到达 j 更短
                // 3. j 还未确定最短路
                if (graph[cur][j] != Integer.MAX_VALUE
                        && minDist[j] > minDist[cur] + graph[cur][j]
                        && !visited[j]) {

                    minDist[j] = minDist[cur] + graph[cur][j];
                }
            }
        }

        // 输出 1 -> N 的最短距离
        if (minDist[N] != Integer.MAX_VALUE) {
            System.out.println(minDist[N]);
        } else {
            // 无法到达
            System.out.println(-1);
        }

        sc.close();
    }
}
```

堆优化版：

```java
import java.util.*;

// 邻接表中的边
class Edge {
    // 边的终点
    int to;

    // 边权
    int val;

    Edge(int to, int val) {
        this.to = to;
        this.val = val;
    }
}

// 自定义 Pair
// first：节点编号
// second：从源点到该节点的当前距离
class Pair<U, V> {
    U first;
    V second;

    Pair(U first, V second) {
        this.first = first;
        this.second = second;
    }
}

// 优先队列比较器
// 按照距离从小到大排序
class MyComparator implements Comparator<Pair<Integer, Integer>> {
    @Override
    public int compare(Pair<Integer, Integer> p1, Pair<Integer, Integer> p2) {
        return Integer.compare(p1.second, p2.second);
    }
}

public class Main {

    // visited[i] = true 表示节点 i 的最短距离已经确定
    public static boolean[] visited;

    // minDist[i] 表示从起点 1 到节点 i 的当前最短距离
    public static int[] minDist;

    // 邻接表
    // graph[u] 存放从 u 出发的所有边
    public static List<Edge>[] graph;

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        // N 个节点，M 条边
        int N = sc.nextInt();
        int M = sc.nextInt();

        // 初始化邻接表
        graph = new ArrayList[N + 1];
        for (int i = 0; i <= N; i++) {
            graph[i] = new ArrayList<>();
        }

        visited = new boolean[N + 1];
        minDist = new int[N + 1];

        // 初始认为所有节点不可达
        Arrays.fill(minDist, Integer.MAX_VALUE);

        // 读入有向边 S -> E，边权为 V
        for (int i = 0; i < M; i++) {
            int S = sc.nextInt();
            int E = sc.nextInt();
            int V = sc.nextInt();

            graph[S].add(new Edge(E, V));
        }

        // 小根堆
        // 每次弹出当前距离源点最近的节点
        PriorityQueue<Pair<Integer, Integer>> pq =
                new PriorityQueue<>(new MyComparator());

        int start = 1;

        // 起点到自己的距离为 0
        minDist[start] = 0;

        // 将起点加入优先队列
        pq.add(new Pair<>(start, 0));

        // 堆优化 Dijkstra 主循环
        while (!pq.isEmpty()) {

            // 取出当前距离最小的节点
            Pair<Integer, Integer> p = pq.poll();

            int cur = p.first;

            // 如果该节点已经确定最短路，跳过
            // 因为优先队列中可能存在同一个节点的旧距离
            if (visited[cur]) {
                continue;
            }

            // 第一次弹出该节点时，它的最短路就确定了
            visited[cur] = true;

            // 遍历 cur 出发的所有边
            for (Edge edge : graph[cur]) {

                // 如果 edge.to 还没有确定最短路
                // 并且经过 cur 到 edge.to 更短
                if (!visited[edge.to]
                        && minDist[cur] + edge.val < minDist[edge.to]) {

                    // 松弛操作
                    minDist[edge.to] = minDist[cur] + edge.val;

                    // 将新的距离加入优先队列
                    pq.add(new Pair<>(edge.to, minDist[edge.to]));
                }
            }
        }

        // 输出 1 -> N 的最短距离
        if (minDist[N] != Integer.MAX_VALUE) {
            System.out.println(minDist[N]);
        } else {
            // 如果 N 不可达，输出 -1
            System.out.println(-1);
        }

        sc.close();
    }
}
```



# 743. 网络延迟时间

## 题目描述

有 `n` 个网络节点，标记为 `1` 到 `n`。

给你一个列表 `times`，表示信号经过 **有向** 边的传递时间。 `times[i] = (ui, vi, wi)`，其中 `ui` 是源节点，`vi` 是目标节点， `wi` 是一个信号从源节点传递到目标节点的时间。

现在，从某个节点 `K` 发出一个信号。需要多久才能使所有节点都收到信号？如果不能使所有节点收到信号，返回 `-1` 。

 

**示例 1：**

![img](./LeetCode--代码随想录(图论).assets/931_example_1.png)

```
输入：times = [[2,1,1],[2,3,1],[3,4,1]], n = 4, k = 2
输出：2
```

**示例 2：**

```
输入：times = [[1,2,1]], n = 2, k = 1
输出：1
```

**示例 3：**

```
输入：times = [[1,2,1]], n = 2, k = 2
输出：-1
```

 

**提示：**

- `1 <= k <= n <= 100`
- `1 <= times.length <= 6000`
- `times[i].length == 3`
- `1 <= ui, vi <= n`
- `ui != vi`
- `0 <= wi <= 100`
- 所有 `(ui, vi)` 对都 **互不相同**（即，不含重复边）

## 图解思路

![image-20260615162511070](./LeetCode--代码随想录(图论).assets/image-20260615162511070.png)

![image-20260615162517527](./LeetCode--代码随想录(图论).assets/image-20260615162517527.png)

## 代码

Dijkstra算法朴素实现：

```java
class Solution {

    // minDist[i]：
    // 从起点 k 到节点 i 的当前最短距离
    int[] minDist;

    // visited[i]：
    // 节点 i 是否已经确定最短路径
    boolean[] visited;

    public int networkDelayTime(int[][] times, int n, int k) {

        // 邻接矩阵
        // graph[u][v] = u -> v 的边权
        int[][] graph = new int[n + 1][n + 1];

        minDist = new int[n + 1];

        // 初始化为无穷大，表示暂时不可达
        Arrays.fill(minDist, Integer.MAX_VALUE);

        visited = new boolean[n + 1];

        // 初始化邻接矩阵
        // Integer.MAX_VALUE 表示两点之间没有边
        for (int i = 0; i <= n; i++) {
            for (int j = 0; j <= n; j++) {
                graph[i][j] = Integer.MAX_VALUE;
            }
        }

        // 建图
        for (int i = 0; i < times.length; i++) {

            int u = times[i][0];
            int v = times[i][1];
            int w = times[i][2];

            graph[u][v] = w;
        }

        // 起点到自己的距离为 0
        minDist[k] = 0;

        // Dijkstra 主循环
        // 每轮确定一个节点的最短路径
        for (int i = 0; i < n; i++) {

            // 当前距离起点最近的未访问节点
            int cur = 0;

            // 当前最小距离
            int minVal = Integer.MAX_VALUE;

            // 在所有未访问节点中
            // 找到距离起点最近的节点
            for (int j = 1; j <= n; j++) {
                if (!visited[j] && minDist[j] < minVal) {
                    minVal = minDist[j];
                    cur = j;
                }
            }

            // 将该节点加入最短路径集合
            visited[cur] = true;

            // 用 cur 去更新其它节点的最短距离
            for (int j = 1; j <= n; j++) {

                // 条件：
                // 1. j 未被访问
                // 2. cur -> j 存在边
                // 3. 经过 cur 到达 j 更短
                if (!visited[j]
                        && graph[cur][j] != Integer.MAX_VALUE
                        && minDist[j] > minDist[cur] + graph[cur][j]) {

                    // 松弛操作（Relax）
                    minDist[j] = minDist[cur] + graph[cur][j];
                }
            }
        }

        // 网络延迟时间：
        // 起点 k 发出的信号到达所有节点所需时间
        // 即所有最短路径中的最大值
        int result = 0;

        for (int i = 1; i <= n; i++) {
            result = Math.max(result, minDist[i]);
        }

        // 如果存在不可达节点
        // max 会变成 Integer.MAX_VALUE
        if (result == Integer.MAX_VALUE) {
            result = -1;
        }

        return result;
    }
}
```

堆优化版Dijkstra：

```java
class Edge {

    // 边的终点
    int to;

    // 边权
    int val;

    Edge(int to, int val) {
        this.to = to;
        this.val = val;
    }
}

class Pair<U, V> {

    // 节点编号
    U first;

    // 从源点到该节点的当前最短距离
    V second;

    Pair(U first, V second) {
        this.first = first;
        this.second = second;
    }
}

// 优先队列比较器
// 距离越小优先级越高（小根堆）
class MyComparator implements Comparator<Pair<Integer, Integer>> {

    @Override
    public int compare(Pair<Integer, Integer> p1,
                       Pair<Integer, Integer> p2) {

        return Integer.compare(p1.second, p2.second);
    }
}

class Solution {

    // minDist[i]
    // 表示源点 k 到节点 i 的当前最短距离
    int[] minDist;

    // visited[i]
    // 表示节点 i 的最短路径是否已经确定
    boolean[] visited;

    public int networkDelayTime(int[][] times, int n, int k) {

        // 邻接表
        // graph[u] 存储从 u 出发的所有边
        List<Edge>[] graph = new ArrayList[n + 1];

        minDist = new int[n + 1];

        // 初始认为所有节点不可达
        Arrays.fill(minDist, Integer.MAX_VALUE);

        visited = new boolean[n + 1];

        // 初始化邻接表
        for (int i = 0; i <= n; i++) {
            graph[i] = new ArrayList<>();
        }

        // 建图
        // times[i] = [u,v,w]
        // 表示 u -> v 的边权为 w
        for (int i = 0; i < times.length; i++) {

            int u = times[i][0];
            int v = times[i][1];
            int w = times[i][2];

            graph[u].add(new Edge(v, w));
        }

        // 小根堆
        //
        // Pair<节点编号, 到源点距离>
        //
        // 堆顶始终是距离源点最近的未处理节点
        PriorityQueue<Pair<Integer, Integer>> pq =
                new PriorityQueue<>(new MyComparator());

        // 起点到自己的距离为 0
        minDist[k] = 0;

        // 起点入堆
        pq.add(new Pair<>(k, 0));

        // Dijkstra 主循环
        while (!pq.isEmpty()) {

            // 取出当前距离最小的节点
            Pair<Integer, Integer> p = pq.poll();

            int cur = p.first;

            // 如果该节点已经确定最短路
            // 直接跳过
            if (visited[cur]) {
                continue;
            }

            // 当前节点最短路确定
            visited[cur] = true;

            // 遍历 cur 的所有邻接边
            for (Edge edge : graph[cur]) {

                // 松弛操作
                //
                // 原路径:
                // k -> edge.to
                //
                // 新路径:
                // k -> cur -> edge.to
                //
                // 如果更短，则更新
                if (!visited[edge.to]
                        && minDist[cur] + edge.val < minDist[edge.to]) {

                    minDist[edge.to] =
                            minDist[cur] + edge.val;

                    // 将更新后的距离加入优先队列
                    pq.add(new Pair<>(
                            edge.to,
                            minDist[edge.to]
                    ));
                }
            }
        }

        // 网络延迟时间
        //
        // 即源点 k 到所有节点最短距离中的最大值
        int result = 0;

        for (int i = 1; i <= n; i++) {
            result = Math.max(result, minDist[i]);
        }

        // 如果存在不可达节点
        // 则某个 minDist[i] 仍为 Integer.MAX_VALUE
        if (result == Integer.MAX_VALUE) {
            return -1;
        }

        return result;
    }
}
```

# 94.城市间货物运输 I

## 题目描述

某国为促进城市间经济交流，决定对货物运输提供补贴。共有 n 个编号为 1 到 n 的城市，通过道路网络连接，网络中的道路仅允许从某个城市单向通行到另一个城市，不能反向通行。



网络中的道路都有各自的运输成本和政府补贴，**道路的权值计算方式为：运输成本 - 政府补贴**。权值为正表示扣除了政府补贴后运输货物仍需支付的费用；权值为负则表示政府的补贴超过了支出的运输成本，实际表现为运输过程中还能赚取一定的收益。



请找出从城市 1 到城市 n 的所有可能路径中，综合政府补贴后的最低运输成本。如果最低运输成本是一个负数，它表示在遵循最优路径的情况下，运输过程中反而能够实现盈利。



**城市 1 到城市 n 之间可能会出现没有路径的情况，同时保证道路网络中不存在任何负权回路。**

输入描述

第一行包含两个正整数，第一个正整数 n 表示该国一共有 n 个城市，第二个整数 m 表示这些城市中共有 m 条道路。 

接下来为 m 行，每行包括三个整数，s、t 和 v，表示 s 号城市运输货物到达 t 号城市，道路权值为 v （单向图）。

输出描述

如果能够从城市 1 到连通到城市 n， 请输出一个整数，表示运输成本。如果该整数是负数，则表示实现了盈利。如果从城市 1 没有路径可达城市 n，请输出 "unconnected"。

输入示例

```
6 7
5 6 -2
1 2 1
5 3 1
2 5 2
2 4 -3
4 6 4
1 3 5
```

输出示例

```
1
```

提示信息

![img](./LeetCode--代码随想录(图论).assets/20240329112127.png)

示例中最佳路径是从 1 -> 2 -> 5 -> 6，路上的权值分别为 1 2 -2，最终的最低运输成本为 1 + 2 + (-2) = 1。



示例 2：

4 2
1 2 -1
3 4 -1

在此示例中，无法找到一条路径从 1 通往 4，所以此时应该输出 "unconnected"。



数据范围：

1 <= n <= 1000；
1 <= m <= 10000;

-100 <= v <= 100;

## 图解思路

![image-20260616213123009](./LeetCode--代码随想录(图论).assets/image-20260616213123009.png)

![image-20260616213130708](./LeetCode--代码随想录(图论).assets/image-20260616213130708.png)

## 代码

```java
import java.util.*;

public class Main {

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        // n：节点数
        // m：边数
        int n = sc.nextInt();
        int m = sc.nextInt();

        // 边集数组
        // graph[i][0] = 起点
        // graph[i][1] = 终点
        // graph[i][2] = 权值
        int[][] graph = new int[m][3];

        // 读入所有边
        for (int i = 0; i < m; i++) {
            int s = sc.nextInt();
            int t = sc.nextInt();
            int v = sc.nextInt();

            graph[i] = new int[]{s, t, v};
        }

        // minDist[i]
        // 表示从源点1到节点i的当前最短距离
        int[] minDist = new int[n + 1];

        // 初始化为无穷大，表示暂时不可达
        Arrays.fill(minDist, Integer.MAX_VALUE);

        // 源点到自身距离为0
        minDist[1] = 0;

        /*
         * Bellman-Ford 算法
         *
         * 第1轮：
         * 求最多经过1条边的最短路
         *
         * 第2轮：
         * 求最多经过2条边的最短路
         *
         * ...
         *
         * 第n-1轮：
         * 求最多经过n-1条边的最短路
         *
         * 一个不包含环的最短路径最多只会经过 n-1 条边，
         * 因此进行 n-1 轮松弛即可得到最终答案。
         */
        for (int i = 1; i < n; i++) {

            // 遍历所有边进行松弛操作
            for (int[] edge : graph) {

                int from = edge[0];
                int to = edge[1];
                int val = edge[2];

                /*
                 * 松弛操作（Relax）
                 *
                 * 如果：
                 * 源点 -> from 已经可达
                 *
                 * 并且：
                 * 源点 -> from -> to
                 * 比当前记录的最短距离更短
                 *
                 * 则更新最短距离
                 */
                if (minDist[from] != Integer.MAX_VALUE
                        && minDist[to] > minDist[from] + val) {

                    minDist[to] = minDist[from] + val;
                }
            }
        }

        // 输出源点1到节点n的最短距离
        if (minDist[n] != Integer.MAX_VALUE) {
            System.out.println(minDist[n]);
        } else {
            // 不可达
            System.out.println("unconnected");
        }

        sc.close();
    }
}
```

SPFA（Bellman-Ford队列优化版本）

```java
import java.util.*;

class Edge {

    // 边的终点
    int to;

    // 边权
    int val;

    Edge(int to, int val) {
        this.to = to;
        this.val = val;
    }
}

public class Main {

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        // n：节点数
        // m：边数
        int n = sc.nextInt();
        int m = sc.nextInt();

        /*
         * 邻接表
         *
         * graph[u]
         * 存储从 u 出发的所有边
         */
        List<Edge>[] graph = new ArrayList[n + 1];

        for (int i = 0; i <= n; i++) {
            graph[i] = new ArrayList<>();
        }

        // 建图
        for (int i = 0; i < m; i++) {

            int s = sc.nextInt();
            int t = sc.nextInt();
            int v = sc.nextInt();

            graph[s].add(new Edge(t, v));
        }

        /*
         * minDist[i]
         *
         * 表示：
         * 从源点1到节点i的当前最短距离
         */
        int[] minDist = new int[n + 1];

        Arrays.fill(minDist, Integer.MAX_VALUE);

        /*
         * SPFA队列
         *
         * 队列中存放：
         * 最近距离发生变化，
         * 需要继续向外传播的节点
         */
        Queue<Integer> que = new ArrayDeque<>();

        /*
         * isInQue[i]
         *
         * 记录节点当前是否已经在队列中
         *
         * 作用：
         * 防止同一个节点重复入队
         */
        boolean[] isInQue = new boolean[n + 1];

        // 源点初始化
        minDist[1] = 0;

        que.add(1);
        isInQue[1] = true;

        /*
         * SPFA 主循环
         */
        while (!que.isEmpty()) {

            // 取出队首节点
            int node = que.poll();

            // 标记为已离开队列
            isInQue[node] = false;

            /*
             * 遍历 node 的所有出边
             */
            for (Edge edge : graph[node]) {

                int to = edge.to;
                int val = edge.val;

                /*
                 * 松弛操作
                 *
                 * 如果：
                 * 1 -> node -> to
                 *
                 * 比当前记录更短
                 *
                 * 则更新最短距离
                 */
                if (minDist[node] != Integer.MAX_VALUE
                        && minDist[to] > minDist[node] + val) {

                    minDist[to] = minDist[node] + val;

                    /*
                     * 既然 to 的最短距离变小了
                     *
                     * 那么它的所有邻居
                     * 也可能因此得到更优答案
                     *
                     * 所以需要把 to 放入队列
                     */
                    if (!isInQue[to]) {

                        que.add(to);

                        isInQue[to] = true;
                    }
                }
            }
        }

        // 输出结果
        if (minDist[n] == Integer.MAX_VALUE) {
            System.out.println("unconnected");
        } else {
            System.out.println(minDist[n]);
        }

        sc.close();
    }
}
```

# 95.城市间货物运输 II

## 题目描述

某国为促进城市间经济交流，决定对货物运输提供补贴。共有 n 个编号为 1 到 n 的城市，通过道路网络连接，网络中的道路仅允许从某个城市单向通行到另一个城市，不能反向通行。



网络中的道路都有各自的运输成本和政府补贴，**道路的权值计算方式为：运输成本 - 政府补贴**。权值为正表示扣除了政府补贴后运输货物仍需支付的费用；权值为负则表示政府的补贴超过了支出的运输成本，实际表现为运输过程中还能赚取一定的收益。



然而，在评估从城市 1 到城市 n 的所有可能路径中综合政府补贴后的最低运输成本时，存在一种情况：**图中可能出现负权回路。**负权回路是指一系列道路的总权值为负，这样的回路使得通过反复经过回路中的道路，理论上可以无限地减少总成本或无限地增加总收益。为了避免货物运输商采用负权回路这种情况无限的赚取政府补贴，算法还需检测这种特殊情况。



请找出从城市 1 到城市 n 的所有可能路径中，综合政府补贴后的最低运输成本。同时能够检测并适当处理负权回路的存在。



**城市 1 到城市 n 之间可能会出现没有路径的情况**

输入描述

第一行包含两个正整数，第一个正整数 n 表示该国一共有 n 个城市，第二个整数 m 表示这些城市中共有 m 条道路。 

接下来为 m 行，每行包括三个整数，s、t 和 v，表示 s 号城市运输货物到达 t 号城市，道路权值为 v。

输出描述

如果没有发现负权回路，则输出一个整数，表示从城市 `1` 到城市 `n` 的最低运输成本（包括政府补贴）。如果该整数是负数，则表示实现了盈利。如果发现了负权回路的存在，则输出 "circle"。如果从城市 1 无法到达城市 n，则输出 "unconnected"。

输入示例

```
4 4
1 2 -1
2 3 1
3 1 -1 
3 4 1
```

输出示例

```
circle
```

提示信息

路径中存在负权回路，从 1 -> 2 -> 3 -> 1，总权值为 -1，理论上货物运输商可以在该回路无限循环赚取政府补贴，所以输出 "circle" 表示已经检测出了该种情况。



数据范围：

1 <= n <= 1000；
1 <= m <= 10000;

-100 <= v <= 100;

## 图解思路

![image-20260618171520421](./LeetCode--代码随想录(图论).assets/image-20260618171520421.png)

## 代码

Bellman-Ford检测负权环路：

```java
import java.util.*;

public class Main{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);

        // 读入节点数 n 和边数 m
        int n = sc.nextInt();
        int m = sc.nextInt();
        
        // 用二维数组存储所有边，graph[i][0]表示起点，[1]表示终点，[2]表示边权
        int[][] graph = new int[m][3];

        // 读入 m 条边的信息
        for(int i=0; i<m; i++){
            int from = sc.nextInt();   // 边的起点
            int to = sc.nextInt();     // 边的终点
            int val = sc.nextInt();    // 边的权重（距离/花费）

            graph[i][0] = from;
            graph[i][1] = to;
            graph[i][2] = val;
        }

        // flag 用于标记图中是否存在负权环
        boolean flag = false;
        
        // minDist[i] 表示从节点 1 到节点 i 的最短距离
        // 数组大小为 n+1，因为节点编号通常从 1 开始
        int[] minDist = new int[n+1];

        // 初始化所有距离为正无穷，表示初始时不可达
        Arrays.fill(minDist, Integer.MAX_VALUE);

        // 起点到自身的距离为 0
        minDist[1] = 0;

        // Bellman-Ford 算法核心：进行 n 轮松弛操作
        // 正常情况下 n-1 轮即可求出最短路，第 n 轮用于检测负权环
        for(int i=0; i<=n; i++){
            if(i<n){
                // 前 n 轮：正常的边松弛操作
                // 对每条边尝试更新最短距离
                for(int j=0; j<m; j++){
                    int from = graph[j][0];  // 边的起点
                    int to = graph[j][1];    // 边的终点
                    int val = graph[j][2];   // 边的权重
                    
                    // 松弛操作：如果 from 可达，且通过 from 到 to 的路径更短，则更新
                    // 注意：这里使用 > 而不是 >=，保证在相等时不更新（避免不必要的操作）
                    if(minDist[from]!=Integer.MAX_VALUE && minDist[to] > minDist[from] + val){
                        minDist[to] = minDist[from] + val;
                    }
                }
            } else{
                // 第 n+1 轮（即 i==n）：检测负权环
                // 如果这一轮还能松弛成功，说明存在从起点可达的负权环
                for(int j=0; j<m; j++){
                    int from = graph[j][0];
                    int to = graph[j][1];
                    int val = graph[j][2];
                    
                    // 如果还能更新距离，说明存在负权环
                    if(minDist[from]!=Integer.MAX_VALUE && minDist[to] > minDist[from] + val){
                        flag = true;  // 标记存在负权环
                    }
                }
            }
        }

        // 根据结果输出
        if(flag){
            // 存在负权环，最短路不存在（可以无限变小）
            System.out.println("circle");
        } else{
            // 不存在负权环，判断终点是否可达
            if(minDist[n]==Integer.MAX_VALUE){
                // 终点不可达
                System.out.println("unconnected");
            } else{
                // 输出从节点 1 到节点 n 的最短距离
                System.out.println(minDist[n]);
            }
        }
        
        sc.close();
    }
}
```

队列优化版：

```java
import java.util.*;

// 边类，存储邻接点 to 和边权 val
class Edge {
    int to;   // 边的终点
    int val;  // 边的权重

    Edge(int to, int val) {
        this.to = to;
        this.val = val;
    }
}

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        // 读入节点数 n 和边数 m
        int n = sc.nextInt();
        int m = sc.nextInt();

        // 邻接表存图，graph[i] 存储从节点 i 出发的所有边
        List<Edge>[] graph = new ArrayList[n + 1];
        for (int i = 0; i <= n; i++) {
            graph[i] = new ArrayList<>();
        }

        // 读入 m 条有向边
        for (int i = 0; i < m; i++) {
            int from = sc.nextInt();  // 起点
            int to = sc.nextInt();    // 终点
            int val = sc.nextInt();   // 权重

            graph[from].add(new Edge(to, val));
        }

        // SPFA 使用队列进行优化松弛
        Queue<Integer> que = new ArrayDeque<>();

        // minDist[i]：从节点 1 到节点 i 的最短距离
        int[] minDist = new int[n + 1];
        // isInQueue[i]：节点 i 是否已在队列中（避免重复入队）
        boolean[] isInQueue = new boolean[n + 1];
        
        // 初始化所有距离为正无穷
        Arrays.fill(minDist, Integer.MAX_VALUE);

        // 起点入队，距离设为 0
        que.add(1);
        minDist[1] = 0;
        isInQueue[1] = true;

        // count[i]：记录节点 i 的入队次数
        // 若某节点入队次数 >= n，说明存在从起点可达的负权环
        int[] count = new int[n + 1];
        boolean flag = false;  // 标记是否存在负权环

        while (!que.isEmpty()) {
            int from = que.poll();     // 取出队首节点
            isInQueue[from] = false;   // 标记已出队

            // 负权环检测：若该节点入队次数达到 n 次，说明存在负权环
            // 原理：最短路径最多包含 n-1 条边，若某节点被松弛 n 次以上，
            //      说明可以通过无限次绕环使路径无限变小
            if (count[from] >= n) {
                flag = true;
                break;
            }

            // 遍历 from 的所有邻接边，尝试松弛
            for (Edge edge : graph[from]) {
                int to = edge.to;
                int val = edge.val;

                // 松弛操作：如果通过 from 到 to 的路径更短，则更新
                if (minDist[to] > minDist[from] + val) {
                    minDist[to] = minDist[from] + val;

                    // 若 to 不在队列中，则入队
                    if (!isInQueue[to]) {
                        que.add(to);
                        isInQueue[to] = true;
                        count[to]++;  // 入队次数加 1
                    }
                }
            }
        }

        // 输出结果
        if (flag) {
            // 存在负权环，最短路不存在（可以无限变小）
            System.out.println("circle");
        } else {
            if (minDist[n] == Integer.MAX_VALUE) {
                // 终点不可达
                System.out.println("unconnected");
            } else {
                // 输出从节点 1 到节点 n 的最短距离
                System.out.println(minDist[n]);
            }
        }

        sc.close();
    }
}
```

# 96.城市间货物运输 III

## 题目描述

某国为促进城市间经济交流，决定对货物运输提供补贴。共有 n 个编号为 1 到 n 的城市，通过道路网络连接，网络中的道路仅允许从某个城市单向通行到另一个城市，不能反向通行。



网络中的道路都有各自的运输成本和政府补贴，**道路的权值计算方式为：运输成本 - 政府补贴。**权值为正表示扣除了政府补贴后运输货物仍需支付的费用；权值为负则表示政府的补贴超过了支出的运输成本，实际表现为运输过程中还能赚取一定的收益。



请计算在最多经过 k 个城市的条件下，从城市 src 到城市 dst 的最低运输成本。

输入描述

第一行包含两个正整数，第一个正整数 n 表示该国一共有 n 个城市，第二个整数 m 表示这些城市中共有 m 条道路。

接下来为 m 行，每行包括三个整数，s、t 和 v，表示 s 号城市运输货物到达 t 号城市，道路权值为 v。

最后一行包含三个正整数，src、dst、和 k，src 和 dst 为城市编号，从 src 到 dst 经过的城市数量限制。

输出描述

输出一个整数，表示从城市 src 到城市 dst 的最低运输成本，如果无法在给定经过城市数量限制下找到从 src 到 dst 的路径，则输出 "unreachable"，表示不存在符合条件的运输方案。

输入示例

```
6 7
1 2 1
2 4 -3
2 5 2
1 3 5
3 5 1
4 6 4
5 6 -2
2 6 1
```

输出示例

```
0
```

提示信息

从 2 -> 5 -> 6 中转一站，运输成本为 0。 

1 <= n <= 1000； 

1 <= m <= 10000; 

-100 <= v <= 100;

## 图解思路

![image-20260618204909910](./LeetCode--代码随想录(图论).assets/image-20260618204909910.png)

## 代码

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        // 读入节点数 n 和边数 m
        int n = sc.nextInt();
        int m = sc.nextInt();

        // 用边集数组存图，graph[i][0]=起点, [1]=终点, [2]=权重
        int[][] graph = new int[m][3];

        for (int i = 0; i < m; i++) {
            int from = sc.nextInt();  // 边的起点
            int to = sc.nextInt();    // 边的终点
            int val = sc.nextInt();   // 边的权重

            graph[i][0] = from;
            graph[i][1] = to;
            graph[i][2] = val;
        }

        // 读入源点 src、终点 dst、最多经过的边数 k
        int src = sc.nextInt();
        int dst = sc.nextInt();
        int k = sc.nextInt();

        // minDist[i]：从 src 到节点 i 的最短距离
        int[] minDist = new int[n + 1];
        Arrays.fill(minDist, Integer.MAX_VALUE);
        minDist[src] = 0;  // 源点到自身距离为 0

        // 核心：最多松弛 k+1 轮（经过 0~k 条边）
        // 注意 i<=k，因为 k 表示"最多经过 k 条边"
        // 第 0 轮：不经过任何边（只确定源点）
        // 第 1 轮：经过 1 条边
        // ...
        // 第 k 轮：经过 k 条边
        for (int i = 0; i <= k; i++) {
            // 关键：用上一轮的结果 prev 来更新当前轮
            // 防止"串联更新"：同轮次中先更新的节点影响后更新的节点
            // 确保每轮只"多走一条边"
            int[] prev = minDist.clone();

            // 遍历所有边尝试松弛
            for (int j = 0; j < m; j++) {
                int from = graph[j][0];  // 边的起点
                int to = graph[j][1];    // 边的终点
                int val = graph[j][2];   // 边的权重

                // 松弛条件：
                // 1. from 可达（prev[from] 不是无穷）
                // 2. 通过 from 到 to 的路径更短
                if (prev[from] != Integer.MAX_VALUE && minDist[to] > prev[from] + val) {
                    minDist[to] = prev[from] + val;
                }
            }
        }

        // 输出结果
        if (minDist[dst] == Integer.MAX_VALUE) {
            // 终点不可达（即使经过 k 条边也无法到达）
            System.out.println("unreachable");
        } else {
            // 输出从 src 到 dst、最多经过 k 条边的最短距离
            System.out.println(minDist[dst]);
        }

        sc.close();
    }
}
```

# 787. K 站中转内最便宜的航班

## 题目描述

有 `n` 个城市通过一些航班连接。给你一个数组 `flights` ，其中 `flights[i] = [fromi, toi, pricei]` ，表示该航班都从城市 `fromi` 开始，以价格 `pricei` 抵达 `toi`。

现在给定所有的城市和航班，以及出发城市 `src` 和目的地 `dst`，你的任务是找到出一条最多经过 `k` 站中转的路线，使得从 `src` 到 `dst` 的 **价格最便宜** ，并返回该价格。 如果不存在这样的路线，则输出 `-1`。

 

**示例 1：**

![img](./LeetCode--代码随想录(图论).assets/cheapest-flights-within-k-stops-3drawio.png)

```
输入: 
n = 4, flights = [[0,1,100],[1,2,100],[2,0,100],[1,3,600],[2,3,200]], src = 0, dst = 3, k = 1
输出: 700 
解释: 城市航班图如上
从城市 0 到城市 3 经过最多 1 站的最佳路径用红色标记，费用为 100 + 600 = 700。
请注意，通过城市 [0, 1, 2, 3] 的路径更便宜，但无效，因为它经过了 2 站。
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2022/03/18/cheapest-flights-within-k-stops-1drawio.png)

```
输入: 
n = 3, edges = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 1
输出: 200
解释: 
城市航班图如上
从城市 0 到城市 2 经过最多 1 站的最佳路径标记为红色，费用为 100 + 100 = 200。
```

**示例 3：**

![img](./LeetCode--代码随想录(图论).assets/cheapest-flights-within-k-stops-2drawio.png)

```
输入：n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 0
输出：500
解释：
城市航班图如上
从城市 0 到城市 2 不经过站点的最佳路径标记为红色，费用为 500。
```

**提示：**

- `2 <= n <= 100`
- `0 <= flights.length <= (n * (n - 1) / 2)`
- `flights[i].length == 3`
- `0 <= fromi, toi < n`
- `fromi != toi`
- `1 <= pricei <= 104`
- 航班没有重复，且不存在自环
- `0 <= src, dst, k < n`
- `src != dst` 

## 图解思路

![image-20260618205135763](./LeetCode--代码随想录(图论).assets/image-20260618205135763.png)

## 代码

```java
class Solution {

    public int findCheapestPrice(int n, int[][] flights, int src, int dst, int k) {

        // minDist[i]
        // 表示当前已知的：
        // 从 src 到节点 i 的最小花费
        int[] minDist = new int[n];

        // 初始化为无穷大，表示暂时不可达
        Arrays.fill(minDist, Integer.MAX_VALUE);

        // 源点到自身花费为 0
        minDist[src] = 0;

        /*
         * Bellman-Ford 限制边数版本
         *
         * 题目要求：
         * 最多经过 k 个中转站
         *
         * 即：
         * 最多经过 k + 1 条边
         *
         * Bellman-Ford 的性质：
         * 第 i 轮松弛结束后，
         * 得到的是“最多经过 i 条边”的最短路
         *
         * 因此需要进行：
         * k + 1 轮松弛
         */
        for (int i = 0; i <= k; i++) {

            /*
             * 保存上一轮结果
             *
             * prev[j]
             * 表示：
             * 最多经过 i 条边到达 j 的最短距离
             *
             * 本轮更新时只能基于上一轮结果进行转移，
             * 防止一轮中连续使用多条边。
             */
            int[] prev = minDist.clone();

            // 遍历所有航班（边）
            for (int j = 0; j < flights.length; j++) {

                int from = flights[j][0];
                int to = flights[j][1];
                int val = flights[j][2];

                /*
                 * Bellman-Ford 松弛操作
                 *
                 * 如果：
                 * src -> from 已经可达
                 *
                 * 并且：
                 * src -> from -> to
                 * 比当前记录的更便宜
                 *
                 * 则更新答案
                 */
                if (prev[from] != Integer.MAX_VALUE
                        && minDist[to] > prev[from] + val) {

                    minDist[to] = prev[from] + val;
                }
            }
        }

        // 如果终点可达
        if (minDist[dst] != Integer.MAX_VALUE) {
            return minDist[dst];
        }

        // 不可达
        return -1;
    }
}
```

