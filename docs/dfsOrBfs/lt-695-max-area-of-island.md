# 695.岛屿的最大面积
题目链接：[传送门](https://leetcode-cn.com/problems/max-area-of-island/)

## 题目描述：
给定一个包含了一些 $0$ 和 $1$ 的非空二维数组`grid`。

一个**岛屿**是由一些相邻的`1`(代表土地) 构成的组合，这里的「相邻」要求两个`1`必须在水平或者竖直方向上相邻。你可以假设`grid`的四个边缘都被`0`（代表水）包围着。

找到给定的二维数组中最大的岛屿面积。(如果没有岛屿，则返回面积为`0`。)

**注意**: 给定的矩阵`grid`的长度和宽度都不超过`50`。

**示例**:
```
[[0,0,1,0,0,0,0,1,0,0,0,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,1,1,0,1,0,0,0,0,0,0,0,0],
 [0,1,0,0,1,1,0,0,1,0,1,0,0],
 [0,1,0,0,1,1,0,0,1,1,1,0,0],
 [0,0,0,0,0,0,0,0,0,0,1,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,0,0,0,0,0,0,1,1,0,0,0,0]]
```
对于上面这个给定矩阵应返回`6`。注意答案不应该是`11` ，因为岛屿只能包含水平或垂直的四个方向的 `1`。

## 解决方案：
- 时间复杂度：$O(m \times n)$
- 空间复杂度：$O(m \times n)$
- 思路：暴力搜索求连通块中最大面积即可！

## AC代码：
```java
class Solution {
	private int m, n, num;
	private int[][] dir = new int[][] { { -1, 0 }, { 0, 1 }, { 1, 0 }, { 0, -1 } };
	public int maxAreaOfIsland(int[][] grid) {
		if (grid == null || (m = grid.length) == 0 || (n = grid[0].length) == 0)
			return 0;
		int res = 0;
		for (int i = 0; i < m; i++) {
			for (int j = 0; j < n; j++) {
				if (grid[i][j] == 1) {
					num = 1;
					grid[i][j] = 0;
					dfs(grid, i, j);
					res = Math.max(res, num);
				}
			}
		}
		return res;
	}
	private void dfs(int[][] grid, int x, int y) {
		for (int i = 0; i < 4; i++) {
			int dx = x + dir[i][0];
			int dy = y + dir[i][1];
			if (check(dx, dy) && grid[dx][dy] == 1) {
				grid[dx][dy] = 0;
				dfs(grid, dx, dy);
				num++;
			}
		}
	}
	private boolean check(int x, int y) {
		return x >= 0 && x < m && y >= 0 && y < n;
	}
}
```