# 200.岛屿数量
题目链接：[传送门](https://leetcode-cn.com/problems/number-of-islands/)

## 题目描述：
给你一个由`'1'`（陆地）和`'0'`（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。

**示例**：

- 输入:

```
11000
11000
00100
00011
```

- 输出: `3`
- 解释: 每座岛屿只能由水平和/或竖直方向上相邻的陆地连接而成。

## 解决方案：
- 时间复杂度：$O(m \times n)$
- 空间复杂度：$O(m \times n)$
- 思路：简单深搜，只要是`'1'`就彻底遍历即可。

## AC代码：
```java
class Solution {
    private int[][] dir = new int[][] {{-1, 0}, {0, 1}, {1, 0}, {0, -1}};
    private int m, n, res;
    public int numIslands(char[][] grid) {
        if(grid == null || (m = grid.length) == 0 || (n = grid[0].length) == 0) return 0;
        res = 0;
        for(int i = 0; i < m; i++)
            for(int j = 0; j < n; j++) 
                if(grid[i][j] == '1') {
                    res++;
                    dfs(i, j, grid);
                }
        return res;
    }
    private void dfs(int x, int y, char[][] grid) {
        for(int i = 0, dx, dy; i < 4; i++) {
            dx = x + dir[i][0];
            dy = y + dir[i][1];
            if(check(dx, dy) && grid[dx][dy] == '1') {
                grid[dx][dy] = '0';
                dfs(dx, dy, grid);
            }
        }
    }
    private boolean check(int x, int y) {
        return x >= 0 && x < m && y >= 0 && y < n;
    }
}
```