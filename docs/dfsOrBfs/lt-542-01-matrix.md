# 542.01矩阵
题目链接：[传送门](https://leetcode-cn.com/problems/01-matrix/)

## 题目描述：
给定一个由`0`和`1`组成的矩阵，找出每个元素到最近的`0`的距离。

两个相邻元素间的距离为`1`。

**注意**:

- 给定矩阵的元素个数不超过`10000`。
- 给定矩阵中至少有一个元素是`0`。
- 矩阵中的元素只在四个方向上相邻: 上、下、左、右。

## 解决方案：
- 时间复杂度：$O(n × m)$
- 空间复杂度：$O(n × m)$
- 思路：深搜。先检查每个`1`周围是否有`0`，若有则将`ans[i][j]`初始化为`1`，否则初始化为最大值`INF`，然后从每个1坐标处开始深搜，每次只要满足`ans[dx][dy]>ans[x][y]+1`，就更新一下坐标`(dx,dy)`的值，然后继续从该位置递归下去，就像覆盖岛屿那样，最后一定能找到距离0的最近距离。

## AC代码：
```java
class Solution {
	private int m, n;
	private int[][] dir = new int[][] { { -1, 0 }, { 0, 1 }, { 1, 0 }, { 0, -1 } };
	private int[][] ans;
	public int[][] updateMatrix(int[][] matrix) {
		if (matrix == null || (m = matrix.length) == 0 || (n = matrix[0].length) == 0)
			return null;
		ans = new int[m][n]; // ans[i][j] 表示坐标(i,j)到附近0的最短距离
		for (int i = 0; i < m; i++)
			for (int j = 0; j < n; j++)
				if (matrix[i][j] > 0) { // 若该位置是1
					if (hasZero(matrix, i, j)) // 且周围有0，则该位置的值设置为1
						ans[i][j] = 1;
					else // 否则设置为无穷大
						ans[i][j] = 10005;
				}
		for (int i = 0; i < m; i++) {
			for (int j = 0; j < n; j++) {
				if (matrix[i][j] == 1) // 有1的位置才深搜
					dfs(matrix, i, j);
			}
		}
		return ans;
	}
	// 判断坐标(i,j)周围是否0
	private boolean hasZero(int[][] matrix, int x, int y) {
		for (int i = 0, dx, dy; i < 4; i++) {
			dx = x + dir[i][0];
			dy = y + dir[i][1];
			if (check(dx, dy) && matrix[dx][dy] == 0)
				return true;
		}
		return false;
	}
	private void dfs(int[][] matrix, int x, int y) {
		for (int i = 0, dx, dy; i < 4; i++) {
			dx = x + dir[i][0];
			dy = y + dir[i][1];
			// 从(x,y)当前最短距离一直向外扩张，向覆盖岛屿那样
			if (check(dx, dy) && ans[dx][dy] > ans[x][y] + 1) {
				ans[dx][dy] = ans[x][y] + 1;
				dfs(matrix, dx, dy);
			}
		}
	}
	private boolean check(int x, int y) {
		return x >= 0 && x < m && y >= 0 && y < n;
	}
}
```