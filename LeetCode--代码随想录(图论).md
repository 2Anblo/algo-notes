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

