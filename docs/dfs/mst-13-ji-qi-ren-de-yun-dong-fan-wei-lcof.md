# 面试题13.机器人的运动范围
## 题目描述：
地上有一个m行n列的方格，从坐标`[0,0]`到坐标`[m-1,n-1]`。一个机器人从坐标`[0, 0]`的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格`[35, 37]`，因为3+5+3+7=18。但它不能进入方格 `[35, 38]`，因为3+5+3+8=19。请问该机器人能够到达多少个格子？

## 解决方案：
- 时间复杂度：$O(m × n)$
- 空间复杂度：$O(m × n)$
- 思路：机器人移动的范围一定是左上角那一坨，有空缺的格子一定走不了，直接暴搜即可！

## AC代码：
```java
class Solution {
	private boolean[][] vis;
	private int[][] dir = new int[][] { { -1, 0 }, { 0, 1 }, { 1, 0 }, { 0, -1 } };
	private int m, n;
	public int movingCount(int m, int n, int k) {
		this.m = m;
		this.n = n;
		vis = new boolean[m][n];
		vis[0][0] = true;
		return dfs(0, 0, k);
	}
	private int dfs(int x, int y, int k) {
		int res = 0; // 记录当前位置可以延申的节点个数
		for (int i = 0; i < 4; i++) {
			int dx = x + dir[i][0];
			int dy = y + dir[i][1];
			if (check(dx, dy) && !vis[dx][dy] && getSum(dx) + getSum(dy) <= k) {
				vis[dx][dy] = true;
				res += dfs(dx, dy, k);
			}
		}
		return res + 1;
	}
	private boolean check(int x, int y) {
		return x >= 0 && y >= 0 && x < m && y < n;
	}
	private int getSum(int x) {
		int res = 0;
		while (x > 0) {
			res += x % 10;
			x /= 10;
		}
		return res;
	}
}
```